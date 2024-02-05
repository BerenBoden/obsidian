![image](https://beren-obsidian-images.imgix.net/26974a5f76f541b7235d2a6b07e9b869.jpeg)
This article will go over how to configure an automated home lab using Proxmox, Packer, Ansible, Terraform and Vault for secrets. This could be used for malware analysis, a SOC lab, performing penetration tests against Windows AD environments and Linux servers, performing penetration tests against deliberately vulnerable web applications, setting up AD environments, testing operating systems, and much more.
## Contents
- [[#Introduction to the home lab project]]
- [[#Configuring PfSense router with two interfaces (isolation & infrastructure)]]
- [[#Using Packer to automate template creation]]
- [[#Configuring VNC connection from the host to the jump-box & configuring RDP to connect from the jump-box to all other internal machines]]
- [[#Configuring Ansible for post-provisioning]]
- [[#Setting up Wazuh for security monitoring & SIEM server]]
- [[#Configuring a DHCP server for the isolation network]]
## Introduction to the home lab project
In order to get started, we need to set up a PfSense router, a Windows Server, and an Ubuntu desktop for the jump-box. If you already have a Packer template for the Ubuntu desktop then you can run that, otherwise it is just as easy to manually install an Ubuntu desktop to run Packer from. 

The jump-box will be used to run all of the Ansible, Terraform, and Packer scripts from. It will also be used to connect to the other machines in our cloud through RDP. 
## Configuring PfSense router with two interfaces (isolation & internet)

## Using Packer to automate template creation
Packer is going to be automating the provisioning of .iso images and creating a template after the provisioning is finished. The reason for this is to ensure that the OS is functioning correctly, the created template verifies that every feature that you have defined in the .pkr.hcl file is working on the newly created OS. Using Packer will save a lot of time if you ever need to quickly create a template in Proxmox from an .iso, you can edit your machines hardware from the .pkr.hcl file rather than doing so manually.

The jump-box will be responsible for running Packer, to [install Packer](https://developer.hashicorp.com/packer/tutorials/docker-get-started/get-started-install-cli) on the jump-box run these commands:
```
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install packer
```
## Configuring VNC connection from the host to the jump-box & configuring RDP to connect from the jump-box to all other internal machines

**TigerVNC**
I will be using TigerVNC to connect to the jump-box through VNC. To install TigerVNC go to the GitHub and download the latest source code `https://github.com/TigerVNC/tigervnc/releases`. 

Now run these commands to extract and run:
```
tar -xzvf "~/Downloads/Source code.tar.gz"
cd "Source code"
make && sudo make install
./binary_name
```

**Connecting to jump-box**
Inside the Proxmox shell, edit the configuration file containing the VM ID of the jump-box you are trying to connect to through VNC `nano /etc/pve/qemu-server/111.conf`. Now add this line to the top of the file, and replace the `PORT_NUMBER` with any port number you wish to connect through:
	`args: -vnc 0.0.0.0:<PORT_NUMBER>`

Then restart the `nfs-kernel-server` to save the changes, and also restart the jump-box itself:
	`systemctl restart nfs-kernel-server`

Now go to TigerVNC from the host machine and connect through the port number on the same network that Proxmox is currently running. `192.168.1.169:<PORT_NUMBER>`. You should now have a remote VNC connection from your host to your jump-box and you can now access the Proxmox web GUI through your jump-box and RDP to any other machine from there as well.

**RDP from jump-box to other machines**
From here, I will be using Remmina for an RDP connection to every other machine from the jump-box.
## Configuring Ansible for post-provisioning

**Initial installation of Ansible**
Ansible will be used for post-provisioning groups of machines from the isolation network. This will be run from the jump-box, Install Ansible on Ubuntu with the following:
```
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

Now make sure the host you are running Ansible from, is able to ping a host on the isolation network. 

**Configuring hosts for Ansible**
Edit the `/etc/ansible/hosts` file to create a group of hosts for Ansible to control. I have added a Windows Server 2019 host that I configured with a NIC on the isolation network.
```
[domaincontrollers]
10.1.99.3

[domaincontrollers:vars]
ansible_user=administrator
ansible_connection=ssh
ansible_password=password
ansible_shell_type=cmd
```

Now navigate to `home-lab/Ansible` and run the command `ansible-playbook domain_controllers` and this will set up a DC on the Windows Server at `10.1.99.3` through Ansible.

I also want to set up some users, GPOs, and another admin, there is a script for this, so I will run `ansible-playbook domain_users.yml`. There are many more Ansible scripts you can run to automate the process of setting up environments, it is up to you what to run next. 
## Setting up Wazuh for security monitoring & SIEM server
I will be running Wazuh from an Ubuntu 22.04 machine, I have configured the netplan as such to assign a static address, both on the isolation and infrastructure network. This is what the `/etc/netplan/00-installer-config.yaml` file should look like:
```
network:
  ethernets:
    ens18:
      dhcp4: false
      addresses:
       - 192.168.99.5/24
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
      routes:
        - to: 0.0.0.0/0
          via: 192.168.99.1
          metric: 99
    ens19:
      dhcp4: false
      addresses:
       - 10.1.99.5/24
      nameservers:
        addresses: [10.1.99.3, 8.8.4.4]
  version: 2
```

To get quickly going with a SIEM server, Wazuh provides a one liner installation command to get up and running.
	`curl -sO https://packages.wazuh.com/4.5/wazuh-install.sh && sudo bash ./wazuh-install.sh -a`
## Configuring a DHCP server for the isolation network