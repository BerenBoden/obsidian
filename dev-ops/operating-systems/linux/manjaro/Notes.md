If correct password is not working run this: `faillock reset`

##### ***Change Arch IP***
To permanently change the IP address of the `eno1` interface on an Arch Linux system, you'll typically use `systemd-networkd`. Here's a step-by-step guide:

1. **Ensure systemd-networkd is Enabled and Running**:
    
    - First, make sure that `systemd-networkd` is enabled:
        
        bashCopy code
        
        `sudo systemctl enable systemd-networkd sudo systemctl start systemd-networkd`
        
2. **Create or Edit Network Configuration File**:
    
    - Go to the `/etc/systemd/network/` directory.
    - Create or edit a file for your interface. For `eno1`, you might name it `20-eno1.network`:
        
        bashCopy code
        
        `sudo nano /etc/systemd/network/20-eno1.network`
        
3. **Configure the IP Address**:
    
    - In the file, set up the configuration for a static IP. Here’s an example configuration:
        
        makefileCopy code
        
        `[Match] Name=eno1  [Network] Address=192.168.1.204/24 Gateway=192.168.1.1  # Replace with your actual gateway DNS=8.8.8.8         # Replace with your preferred DNS`
        
    - Replace `192.168.1.204/24` with the desired static IP address and subnet. Adjust the Gateway and DNS to your network's requirements.
4. **Apply the Changes**:
    
    - Save and close the file.
    - Restart `systemd-networkd` to apply the changes:
        
        bashCopy code
        
        `sudo systemctl restart systemd-networkd`
        
5. **Verify the Configuration**:
    
    - Use `ip addr show eno1` to verify that the new IP address has been assigned to `eno1`.