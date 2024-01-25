## Preparing the hard disk (UEFI with encryption)

### See partitions/drives on the system (find the name of your hard drive)

```
 fdisk -l
```

### Start the partitioner (fdisk)

```
 fdisk /dev/<DEVICE> (substitute <DEVICE> for your device name, example: /dev/sda or /dev/nvme0n1)
```

### Show current partitions

```
 p
```

### Create EFI partition

```
 g (to create an empty GPT partition table)
 n
 enter
 enter
 +500M
 t
 1 (For EFI)
```

### Create boot partition

```
 n
 enter
 enter
 +500M
```

### Create LVM partition

```
 n
 enter
 enter
 enter
 t
 enter
 30
```

### Show current partitions again

```
 p
```

### Finalize partition changes

```
 w
```

### Format the EFI partition

```
 mkfs.fat -F32 /dev/<DEVICE PARTITION 1> (for example: /dev/sda1)
```

### Format the boot partition

```
 mkfs.ext4 /dev/<DEVICE PARTITION 2> (for example: /dev/sda2)
```

### Set up encryption

```
 cryptsetup luksFormat /dev/<DEVICE PARTITION 3>
 cryptsetup open --type luks /dev/<DEVICE PARTITION 3> lvm
```

### Set up lvm

```
 pvcreate --dataalignment 1m /dev/mapper/lvm
 vgcreate volgroup0 /dev/mapper/lvm
 lvcreate -L 30GB volgroup0 -n lv_root
 lvcreate -l 250GB volgroup0 -n lv_home (or instead of "-L 250GB", use "-l 100%FREE" to use all the remaining space).
 modprobe dm_mod
 vgscan
 vgchange -ay
```

### Format the root partition

```
mkfs.ext4 /dev/volgroup0/lv_root
```

### Mount the root partition

```
mount /dev/volgroup0/lv_root /mnt
```

### Create the boot partition mount directory

```
mkdir /mnt/boot
```

### Mount the boot partition

```
mount /dev/<DEVICE PARTITION 2> /mnt/boot
```

### Format the home partition

```
mkfs.ext4 /dev/volgroup0/lv_home
```

### Create the home partition mount point

```
mkdir /mnt/home
```

### Mount the home volume

```
mount /dev/volgroup0/lv_home /mnt/home
```

### Create the /etc dirctory

```
mkdir /mnt/etc
```

### Create the /etc/fstab file

```
genfstab -U -p /mnt >> /mnt/etc/fstab
```

### Check the /etc/fstab file

```
cat /mnt/etc/fstab
```

# Install Arch Linux

### Install Arch Linux base packages

```
pacstrap -i /mnt base
```

### Access the in-progress Arch installation

```
arch-chroot /mnt
```

### Install a kernel and headers

```
pacman -S linux linux-headers
```

For LTS:

```
pacman -S linux-lts linux-lts-headers
```

Or both:

```
pacman -S linux linux-lts linux-headers linux-lts-headers
```

### Install a text editor

```
pacman -S nano
```

### Install optional packages

```
pacman -S base-devel openssh
```

### Enable OpenSSH if you’ve installed it

```
systemctl enable sshd
```

### Install packages for networking

```
pacman -S networkmanager wpa_supplicant wireless_tools netctl
```

### Install dialog (required for wifi-menu)

```
pacman -S dialog
```

### Enable networkmanager

```
systemctl enable NetworkManager
```

### Add LVM support

```
pacman -S lvm2
```

### Edit /etc/mkinitcpio.conf

```
nano /etc/mkinitcpio.conf
```

On the “HOOKS” line, add support for lvm2 and optionally encryption.

### unencrypted hard disk:

Add “lvm2” in between “block” and “filesystems”

### encrypted hard disk:

Add “encrypt lvm2” in between “block” and “filesystems”

It should look similar to the following (don’t copy this line in case they change it, but just add the required new items):

```
HOOKS=(base udev autodetect modconf block encrypt lvm2 filesystems keyboard fsck)
```

### Create the initial ramdisk for the main kernel

```
mkinitcpio -p linux
```

### Create the initial ramdisk for the LTS kernel (if you installed it)

```
mkinitcpio -p linux-lts
```

### Uncomment the line from the /etc/locale.gen file that corresponds to your locale

```
nano /etc/locale.gen (uncomment en_US.UTF-8)
```

### Generate the locale

```
locale-gen
```

### Set the root password

```
passwd
```

### Create a user for yourself

```
useradd -m -g users -G wheel <username>
```

### Set your password

```
 passwd <username>
```

### Install sudo (may already be installed)

```
pacman -S sudo

```

### Allow users in the ‘wheel’ group to use sudo

```
EDITOR=nano visudo
```

Uncomment:

```
%wheel ALL=(ALL) ALL
```

# Setting up GRUB

GRUB is the bootloader that was used in the video. Follow ONE of the following sections, depending on whether you are using UEFI, encryption, etc

### Installing GRUB for non-UEFI, with no encryption

### Install the required packages for GRUB:

```
pacman -S grub dosfstools os-prober mtools
```

# Install GRUB:

### Installing GRUB for UEFI, with LUKS disk encryption

### Install the required packages for GRUB:

```
pacman -S grub efibootmgr dosfstools os-prober mtools
```

### Create the EFI directory:

```
mkdir /boot/EFI
```

### Mount the EFI partition:

```
mount /dev/<DEVICE PARTITION 1> /boot/EFI
```

### Install GRUB:

```
 grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck
```

### Create the locale directory for GRUB

```
 mkdir /boot/grub/locale
```

### Copy the locale file to locale directory

```
cp /usr/share/locale/en\\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo
```

### Set up GRUB to be able to unlock the disk

Open the defaulg config file for GRUB:

```
nano /etc/default/grub
```

Uncomment:

```
GRUB_ENABLE_CRYPTODISK=y
```

Add cryptdevice=<PARTUUID>:volgroup0 to the GRUB_CMDLINE_LINUX_DEFAULT line If using standard device naming, the option will look similar this (be sure to adjust the device name):

```
cryptdevice=/dev/sda3:volgroup0:allow-discards quiet
```

### Generate GRUB’s config file

```
 grub-mkconfig -o /boot/grub/grub.cfg
```

# Testing the installation

### Check the /etc/fstab file to make sure it includes all the right partitions

```
 cat /etc/fstab
```

You should have a mountpoint for all of the partitions that were created.

### Moment of truth: Reboot your machine

### Exit the chroot environment

```
exit
```

### Unmount everything (some errors are okay here)

```
umount -a
```

### Reboot the machine

```
reboot
```

# Post-Install Tweaks/Enhancements

### Create swap file

```
dd if=/dev/zero of=/swapfile bs=1M count=2048 status=progress
chmod 600 /swapfile
mkswap /swapfile
```

### Back up the /etc/fstab file

```
 cp /etc/fstab /etc/fstab.bak
```

### Add the swap file to the /etc/fstab file

```
 echo '/swapfile none swap sw 0 0' | tee -a /etc/fstab
```

# Set time zone

List time zones:

```
timedatectl list-timezones
```

Set your time zone:

```
timedatectl set-timezone America/Detroit
```

Enable time synchronization via systemd:

```
systemctl enable systemd-timesyncd
```

# Set the hostname

Consider setting the hostname of your new installation. You can do so with the following command:

```
hostnamectl set-hostname myhostname
```

Also, make the same change in /etc/hosts:

```
nano /etc/hosts
```

Example lines to add:

```
127.0.0.1 localhost
127.0.1.1 myhostname
```

### Install CPU Microde files (AMD CPU)

```
pacman -S amd-ucode
```

### Install CPU Microde files (Intel CPU)

```
pacman -S intel-ucode
```

### Install Xorg if you plan on having a GUI

```
pacman -S xorg-server
```

### Install 3D support for Intel or AMD graphics

If you have an Intel or AMD GPU, install the mesa package:

```
pacman -S mesa
```

### Install Nvidia Driver packages if you have an Nvidia GPU

```
pacman -S nvidia nvidia-utils
```

Note: Install nvidia-lts if you’ve installed the LTS kernel:

```
pacman -S nvidia-lts
```

### Install Virtualbox guest packages

If you’re installing Arch inside a Virtualbox virtual machine, install these packages:

```
pacman -S virtualbox-guest-utils xf86-video-vmware
```