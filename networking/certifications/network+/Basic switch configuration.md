This packet tracer scenario includes setting up a hostname and an IP address, MOTD banner, MD5 hashed passwords, and telnet access to a Cisco catalyst 2960 switch from a remote computer. This is a lab from `www.packettracernetwork.com`.

I will be using a Cisco command cheat sheet to reference if I can not remember a certain command.

As always, I will first enable the switch and configure the terminal, this will be done from a laptop connected on the same LAN as the switch.
```
Switch>enable
Switch#configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
Switch(config)#
```

Next I will set the hostname to `LOCAL-SWITCH`.
```
Switch(config)#hostname LOCAL-SWITCH
LOCAL-SWITCH(config)#
```

Now I want to set up an MOTD banner, so it will display "Unauthorized access is forbidden."
```
LOCAL-SWITCH(config)#banner motd #
Enter TEXT message.  End with the character '#'.
unauthorized access is forbidden
#
```

As you can see here, once I exit, and come back to the switch, the MOTD banner will now be displayed.
```
LOCAL-SWITCH>exit
LOCAL-SWITCH con0 is now available
Press RETURN to get started.
unauthorized access is forbidden
```

After this I am going to set a password on the switch for privileged access, there are numerous ways to set a password, but the lab specifically requires it to be MD5 hashed. So, I will use this command, as it automatically sets the password hash as MD5.
```
enable secret $ecR3t35
```

Now I will exit the switch, and come back and it will require a password.
```
LOCAL-SWITCH>exit
LOCAL-SWITCH con0 is now available
Press RETURN to get started.
unauthorized access is forbidden
LOCAL-SWITCH>enable
Password:
```

I would like to prevent casual viewing of passwords, from a configuration file or VTY (Virtual Terminal Lines) lines, and auxiliary lines. The `service password-encryption` command is used to add a basic level of encryption to plain text passwords in the Cisco device configuration, making it less trivial for someone to read these passwords directly from the configuration file.
```
LOCAL-SWITCH(config)#service password-encryption
```

Now I will enable console access for accessing the switch through its console port. This is a physical management port that provides out-of-band access to the switch. The console password is used when connecting directly to the switch through a console cable. It provides basic security for switch access.

Purpose of Console Access:
- The console port is typically used for initial configuration of the device, recovery from errors, or other direct management tasks.
- It provides a reliable way to access the device when remote network-based access is not possible or not working.
```
LOCAL-SWITCH(config)#line con 0
LOCAL-SWITCH(config-line)#password console
LOCAL-SWITCH(config-line)#logging synchronous
LOCAL-SWITCH(config-line)#login
LOCAL-SWITCH(config-line)#history size 15
LOCAL-SWITCH(config-line)#exec-timeout 6 45
```

There are many things going on here, let me explain each command.
1. `LOCAL-SWITCH(config)#line con 0`:
- This command accesses the configuration settings for the console line (con 0). The console line is the physical port on the switch/router used for direct device management via a console cable.
2. `LOCAL-SWITCH(config-line)#password console`:
- This sets the password to "console" for accessing the console line. This password is required when you connect to the switch via the console port.
3. `LOCAL-SWITCH(config-line)#logging synchronous`:
- This command synchronizes logging messages with command line input. It prevents console logging messages from interrupting the command you're typing by re-displaying the command line after a log message is shown.
4. `LOCAL-SWITCH(config-line)#login`:
- This enables password checking on the console line. When this is configured, the password set by the `password` command must be entered to access the console line.
5. `LOCAL-SWITCH(config-line)#history size 15`:
- This command sets the command history buffer size to 15 commands on the console line. It allows you to recall up to 15 of the most recent commands entered.
6. `LOCAL-SWITCH(config-line)#exec-timeout 6 45`:
- This sets the EXEC timeout period, which is the time of inactivity after which the console session will be automatically logged out. The time is set to 6 minutes and 45 seconds. If there is no input for this duration, the session will close, requiring a re-login.+

After configuring console access, I will setup Telnet access so that the remote computer can access the switch.
```
LOCAL-SWITCH(config)#line vty 0 15
LOCAL-SWITCH(config-line)#exec-timeout 8 20
LOCAL-SWITCH(config-line)#password ciscotelnet
LOCAL-SWITCH(config-line)#logging synchronous
LOCAL-SWITCH(config-line)#login
LOCAL-SWITCH(config-line)#history size 15
```
1. `line vty 0 15`:
- This command enters the configuration mode for the vty lines 0 through 15.
- Vty lines are used for remote management of the device (like Telnet or SSH).
- `0 15` specifies that the configuration will apply to 16 lines (from 0 to 15).
2. `exec-timeout 8 20`:
- This sets the timeout for the vty lines.
- The device will log out users who are idle for the specified time.
- `8 20` means 8 minutes and 20 seconds of inactivity before automatic logout.
3. `password ciscotelnet`:
- This sets the password for accessing the vty lines.
- Users must enter this password to access the device remotely.
- `ciscotelnet` is the password being set in this case.
4. `logging synchronous`:
- This command synchronizes log messages with command-line input.
- It prevents log messages from interrupting your typing.
- It's useful for improving the readability of command-line interface interaction.
5. `login`:
- This command enables password checking on the vty lines.
- It ensures that remote users are prompted for the password set earlier.
- Without this command, the password would be set but not required for login.
6. `history size 15`:
- This command sets the size of the command history buffer for the current terminal session.
- `15` specifies that the last 15 commands can be recalled.
- It's useful for navigating previously entered commands.
### *Resources*
https://www.netwrix.com/cisco_commands_cheat_sheet.html
https://www.packettracernetwork.com/labs/lab1-basicswitchsetup.html