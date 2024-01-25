##### ***What is SMB?***
The SMB protocol operates at layer 7, it is a client-server communication protocol that is used as a medium for sharing data on a network, and supports [[Named pipes & Inter-process communication]] allowing for more complex operations like message-based communication between different processes, potentially on different networked machines.

SMB operates over ports 139 ([[NETBios - 139 TCP]] over TCP/IP) and 445 (Direct over TCP/IP).
##### ***Enumerating shares with `enum4linux`***
Run `enum4linux` to check for a vulnerable SMB server. `enum4linux -a 10.10.202.38`

Use `smbclient` to see the SMB server.
```
smbclient -L //<ip>
```

Use `smbclient` to try and connect to the server.
```
smbclient [//10.10.150.235/smb](<https://10.10.150.235/smb>) -U Anonymous -p 139
```

Recursively download everything from the SMB server.
```
smbclient //10.10.150.235/profiles -U Anonymous -p 139 -c 'recurse ON; prompt OFF; mget *'
```

Once inside SMB you can Download a file. `get "filename.txt" /tmp/temp_file.txt`
Download a folder.
```
recurse ON
prompt OFF
mget folder_name
```
##### ***Enumerating shares with Nmap***
You can use Nmap to enumerate SMB shares with this command:
```
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.28.128
```

The `--script` option is using two NSE (Nmap Scripting Engine) scripts.
- `smb-enum-shares.nse`: Enumerates shared drives available on the target system.
- `smb-enum-users.nse`: Enumerates user accounts available on the target system.

The result may look something like this, in this case I have 3 shares available:
```
PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-enum-shares: 
|   account_used: guest
|   \\10.10.28.128\IPC$: 
|     Type: STYPE_IPC_HIDDEN
|     Comment: IPC Service (kenobi server (Samba, Ubuntu))
|     Users: 2
|     Max Users: <unlimited>
|     Path: C:\tmp
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.28.128\anonymous: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\home\kenobi\share
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.28.128\print$: 
|     Type: STYPE_DISKTREE
|     Comment: Printer Drivers
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\var\lib\samba\printers
|     Anonymous access: <none>
|_    Current user access: <none>
```

You can connect to it by using the `smbclient //10.10.28.128/anonymous` command.

Use either of these commands to download the items from the SMB share to your current machine:
- `smbget -R smb://10.10.28.128/anonymous`
- `smbclient //10.10.28.128/anonymous -p 445 -c 'recurse ON; prompt OFF; mget *'`