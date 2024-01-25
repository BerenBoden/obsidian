**Enable VNC connection for machine**
Listens on port 5000, edit the `/etc/pve/qemu-server/<vm-id>.conf` file and choose a number of the 10s value, this will listen on 5920:
```
args: -vnc 0.0.0.0:20
```

Then stop and start the VM directly from proxmox

**Initial Proxmox storage set up:** `lvremove /dev/pve/data`

`lvresize -l +100%FREE /dev/pve/root`

`resize2fs /dev/mapper/pve-root`

**Proxmox server is having trouble resolving domain names (e.g., '[ftp.nz.debian.org](http://ftp.nz.debian.org)') to their corresponding IP addresses. This can be due to a misconfigured DNS resolver or a network issue:**

To fix the issue, follow these steps:

1. Check your current DNS settings in the **`/etc/resolv.conf`** file:
    
    `cat /etc/resolv.conf`
    
    You should see at least one **`nameserver`** entry, such as **`nameserver 8.8.8.8`** (Google's public DNS server).
    
2. If you don't see any **`nameserver`** entries or if the existing ones seem incorrect, you can use a text editor to modify the **`/etc/resolv.conf`** file and add a public DNS server, like Google's:
    
    `echo "nameserver 8.8.8.8" | tee -a /etc/resolv.conf echo "nameserver 8.8.4.4" | tee -a /etc/resolv.conf`
    
    This will append Google's public DNS servers to your resolver configuration.
    
3. Save the changes and exit the text editor.
    
4. Restart the networking service:
    
    `systemctl restart networking`
    
5. Test the DNS resolution with a tool like **`ping`** or **`dig`**:
    
    `ping -c 3 ftp.nz.debian.org`
    
    If the command returns a response, the DNS resolution issue should be resolved, and you can retry running **`apt update`**.
    

Note that changes made directly to **`/etc/resolv.conf`** may be temporary, as the file can be overwritten by the system or the network manager. To make the changes persistent, you may need to update your network configuration files or your DHCP client configuration, depending on your system setup.

The "401 Unauthorized" error indicates that you're trying to access the Proxmox Enterprise repository, but you don't have a valid subscription or your subscription credentials are not correctly set up.

If you don't have a Proxmox subscription, you can use the Proxmox No-Subscription Repository instead of the Enterprise repository. To do this, follow these steps:

Open the /etc/apt/sources.list.d/pve-enterprise.list file in a text editor: `nano /etc/apt/sources.list.d/pve-enterprise.list` Comment out the existing line by adding a # at the beginning of the line:

#`deb <https://enterprise.proxmox.com/debian/pve> bullseye pve-enterprise`

Save the changes and exit the text editor.

Create a new file called pve-no-subscription.list in the /etc/apt/sources.list.d/ directory: nano /etc/apt/sources.list.d/pve-no-subscription.list Add the following line to the new file: `deb <http://download.proxmox.com/debian/pve> bullseye pve-no-subscription` Save the changes and exit the text editor.

Update your package lists: `apt update`