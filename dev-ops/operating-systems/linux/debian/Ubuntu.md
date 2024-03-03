### *Using the `find` command*
**Find All Files in Current Directory** 
`.` searches in the current directory and its sub-directories.
```bash
find .
``` 

**Find Files by Name**
`-name` searches for files whose names match the given pattern.
```bash
find /path/to/search -name "filename"
```

**Find Files by Type**
`-type f` searches for regular files.
```bash
find /path/to/search -type f
```

**Find Directories by Name**
`-type d` searches for directories.
```bash
find /path/to/search -type d -name "dirname"
```

**Find and Remove Files**
- `-exec` executes a command on each found file.
- `rm {} \;` removes each found file.
```bash
find /path/to/search -name "*.tmp" -exec rm {} \;
```

**Find Files Modified in the Last N Days**
`-mtime -7` searches for files modified in the last 7 days.
```bash
find /path/to/search -mtime -7
```

**Find Files Bigger Than N Size**
`-size +1M` searches for files larger than 1 megabyte.
```bash
find /path/to/search -size +1M
```

**Find Files with Specific Permissions**
`-perm 0644` searches for files with `0644` permissions.
```bash
find /path/to/search -type f -perm 0644
```

**Find Files Owned by User**
`-user` searches for files owned by the specified user.
```bash
find /path/to/search -user username
```

**Find and Print Only Filenames**
`-printf "%f\n"`: Prints only the names of the found files, one per line.
```bash
find /path/to/search -type f -printf "%f\n"
```

**Find and Replace**
- `find .` search in the current directory and its sub-directories.
- `-type f` look for files only.
- `-exec ... {} +` for each file found, execute the following command.
- `sed -i.bak 's/X-Header/X-Contact/g'` use `sed` to search and replace all occurrences of `X-Header` with `X-Contact`.
- `-i.bak` edit files in-place, and create a backup with the `.bak` extension.
```bash
find . -type f -exec sed -i.bak 's/X-Header/X-Contact/g' {} +
```
### *Wiping a Linux machine*
Run `lsblk` to see which to disk to clear, then run:
```
sudo dd if=/dev/zero of=/dev/vda bs=4k
```
### *Using the `du` command*
See the size of a folder in human-readable format
```bash
du -sh /path/to/folder
```
### *Format USB from command line*
Reformat drive to FAT32
```
**Unmount All Partitions on the Drive**
sudo umount /dev/sdc1
sudo umount /dev/sdc2
sudo umount /dev/sdc3
sudo umount /dev/sdc4

**Delete the Partition Table**
sudo parted /dev/sdc mklabel msdos

**Create a New Partition and Format as FAT32**
sudo mkfs.vfat -I /dev/sdc
```
### *IP commands*

### *Find running services*
Find running services on port 443
```
sudo lsof -i :443
sudo netstat -tuln | grep 443
```
### *Fix GO installation error*
Edit .bashrc file:
```
nano .bashrc
```

append the following lines at the bottom:
```
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
```

Finally type following in terminal:
```
 source .bashrc
```
### *Ubuntu VM in Proxmox showing black screen with PCI-pass through*
Create the VM without GPU pass through at first, then run these commands.
```
1. sudo apt-get install --reinstall ubuntu-desktop gnome-shell ubuntu-gnome-desktop unity [reinstalls all post display manager stuff]
2. sudo apt-get install --reinstall nvidia-common
3. sudo apt-get install xserver-xorg-video-nouveau
4. sudo apt install nvidia-settings
5. sudo nano /etc/default/grub
//change GRUB_CMDLINE_LINUX_DEFAULT="quiet splash" to be GRUB_CMDLINE_LINUX_DEFAULT="quiet splash nouveau.modeset=0"
6. sudo update-grub
7. sudo reboot
```

Once you have setup GPU pass through for ubuntu VM on proxmox, use `sudo ubuntu-drivers autoinstall` to install the drivers.
### *Resizing disks, LVMs & partitioning*
**lvextend**
This command is used to extend the logical volume managed by LVM (Logical Volume Manager). The `-l +100%FREE` option specifies that you want to use all of the free space available in the volume group to extend this logical volume. After running this command, the logical volume will have more raw block storage, but the file system itself won't yet know how to use this new space.
```
lvextend -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv
```

**resize2fs**
This command is used to resize the file system that resides on the logical volume so that it can actually use the new space that was added by `lvextend`. Until you run `resize2fs`, the file system will still report the old size because it doesn't yet know that it has been given more space to use.
```
resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```

**Resize LVM**
Find the LVM you need to resize with `lsblk`:
```
ubuntu@ubuntu:~$ lsblk
NAME                      MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0                       7:0    0  63.5M  1 loop /snap/core20/2015
loop1                       7:1    0 111.9M  1 loop /snap/lxd/24322
loop2                       7:2    0  49.8M  1 loop /snap/snapd/18357
loop3                       7:3    0  40.9M  1 loop /snap/snapd/20290
loop4                       7:4    0  63.3M  1 loop /snap/core20/1822
sda                         8:0    0   150G  0 disk 
├─sda1                      8:1    0     1M  0 part 
├─sda2                      8:2    0     2G  0 part /boot
└─sda3                      8:3    0   148G  0 part 
  └─ubuntu--vg-ubuntu--lv 253:0    0    74G  0 lvm  /
sr0                        11:0    1   1.8G  0 rom  
```

Boot into recovery mode. Then mount the directory you need to resize, resize with `lvresize` and use `resize2fs` to use the new space that was added by `lvextend`: 
```
mount -o remount,rw /
sudo lvresize -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv
sudo resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```

**Add second drive to existing LVM**
Use `fdisk` or p`arted` to create a new partition. Suppose the new drive is `/dev/sdb`, and you create a partition `/dev/sdb1`.
```
fdisk /dev/sdb
```

Inside `fdisk` run these commands;
- Create a new partition:
- `Press n for a new partition`
- Choose partition type:
- `Press p for a primary partition`
- Choose partition number:
- `Press Enter to accept the default`
- Choose first sector:
- `Press Enter to accept the default first sector`
- Choose last sector:
- `Press Enter to accept the default last sector`
- Convert this new partition into a physical volume for use with LVM.
- `pvcreate /dev/sdb1`
- Write changes:
- `Press w to write changes`

Next locate the volume group to add the new partition to.
```
sudo vgs
  VG        #PV #LV #SN Attr   VSize    VFree
  ubuntu-vg   1   1   0 wz--n- <148.00g    0 
```

Add this physical volume to your existing volume group (`ubuntu-vg`).
```
vgextend ubuntu-vg /dev/sdb1
```

Now extend your logical volume.
```
lvextend -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv
```

Resize the file system with `resize2fs`.
```
resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```
### *OpenSSH*
Install OpenSSH with:
```
sudo apt install openssh-server
```

Uncomment the following line from the `/etc/ssh/sshd_config` file to enable password authentication:
```
PasswordAuthentication yes
```

Restart OpenSSH:
```
sudo systemctl restart sshd
```

### *Lock failure*
**Check if APT or DPKG Processes are Still Running**
Before manually removing the lock, it's important to check if there are any ongoing APT processes that might be using it.
```
ps aux | grep -i apt
```

This command will list any running processes related to APT. If you find any, it's safer to wait for them to complete. If these processes are stuck or not making any progress, you may consider stopping them.
    
**Kill the Stuck APT Processes**
If there are stuck APT processes, you can kill them using their process IDs (PIDs). In your case, the PID is 2127.
```
sudo kill -9 2127
```

Replace `2127` with the PID of the process you want to kill. Be cautious with this command, as killing processes can have unintended side effects.
    
**Remove the Lock Files**
If there are no active APT processes, or if you have killed the stuck processes, you can safely remove the lock files.
```
sudo rm /var/lib/dpkg/lock-frontend sudo rm /var/cache/apt/archives/lock sudo rm /var/lib/apt/lists/lock
```

**Reconfigure the Package Database**
After removing the lock files, it's a good practice to reconfigure the package database in case it was left in an inconsistent state.
```
sudo dpkg --configure -a
```
### *Enable Bluetooth on start up*
**Create a Script for Bluetooth Control**
Save the following script to a file like `/usr/local/bin/bluetooth-authorization.sh`. Make sure to replace `SERVICE_UUID` with the appropriate UUID for your service.
```
#!/bin/bash  
echo "Agent registered" | bluetoothctl echo -e "yes\n" | bluetoothctl authorize $SERVICE_UUID
```    

Make sure to give the script execute permission:
```
chmod +x /usr/local/bin/bluetooth-authorization.sh
```
    
**Create a Systemd Service File**
Create a systemd service file at `/etc/systemd/system/bluetooth-authorization.service` with the following content. This file tells systemd to run the script after the Bluetooth service has started.
```
[Unit] Description=Bluetooth Authorization Service After=bluetooth.service Requires=bluetooth.service  [Service] Type=simple ExecStart=/usr/local/bin/bluetooth-authorization.sh  [Install] WantedBy=multi-user.target
```

**Enable and Start the Service**
Enable the service so that it starts on boot, and then start it:
`sudo systemctl enable bluetooth-authorization.service sudo systemctl start bluetooth-authorization.service`

### *Notes*
- Table 5­1: Octal and Binary Representations of Permissions. 
    ```bash
    000 0 ---
    001 1 --x
    011 3 -wx
    010 2 -w
    100 4 r--
    101 5 r-x
    110 6 rw
    111 7 rwx
    ```

**Delete goLogin**
```
rm -r /home/user/.config/GoLogin
```
- Arp scan.
	`sudo arp-scan --interface=eno1 --localnet`
- Atheros ALFA firmware.
 [https://github.com/qca/open-ath9k-htc-firmware](https://github.com/qca/open-ath9k-htc-firmware)
- Find all files related to an application and remove.

 ```jsx
sudo find /usr/local/bin /usr/local/etc /usr/local/man /usr/local/share -iname '*hydra*' -exec rm -r {} \\;

    
- Rename everything in a folder to be url-friendly.
    
    ```jsx
    for folder in *; do mv "$folder" "$(echo "$folder" | tr '[:upper:]' '[:lower:]' | tr ' ' '-')"; done
    ```
    
- Find running ports and kill.
    
    ```jsx
    sudo lsof -i :5173
    sudo kill PID
    ```
    
- Setup pfsense with ivpn
    
    - [https://www.ivpn.net/setup/router/pfsense-wireguard/](https://www.ivpn.net/setup/router/pfsense-wireguard/)
- Copy folder and exlcude with rsync:
    
    ```jsx
    rsync -avz --exclude 'recon-runner/node_modules' -e ssh recon-runner [bug@170.64.166.223](<mailto:bug@170.64.166.223>):/home/bug/bounty/.
    ```
    **Cant update or install anything kali:**
[https://unix.stackexchange.com/questions/421821/invalid-signature-for-kali-linux-repositories-the-following-signatures-were-i](https://unix.stackexchange.com/questions/421821/invalid-signature-for-kali-linux-repositories-the-following-signatures-were-i)
**Virtualbox left side unresponsive:**
`kill -15 $(ps aux | grep "VBoxClient --draganddrop" | grep "Sl" | awk '{print $2;}')`
*Squid Proxy*
- **Add authentication.**
    ```jsx
    auth_param basic program /usr/lib/squid3/basic_ncsa_auth /etc/squid/passwords
    auth_param basic realm proxy
    http_access allow authenticated
    ```
- **Add ip address source**
    ```jsx
    acl localnet src <ivpn address>
    ```
    

[Squid-Proxy](https://www.notion.so/Squid-Proxy-0fc17292fa0e476985b84f8476ebe8c7?pvs=21)
