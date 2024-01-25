##### ***Tricks***
To see the taskbar when doing an RDP session, run this command
`cd /home/youruser echo "mate-session" > .xsession`

Mount guest additions iso from `/dev/sr0`
```
##Find what mount point is called
lsblk
NAME   MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
sda      8:0    0  48G  0 disk 
└─sda1   8:1    0  48G  0 part /home
sr0     11:0    1  51M  0 rom 

sudo mkdir /media/iso
sudo mount /dev/sr0 /media/iso
chmod -R a+w /media/iso
```