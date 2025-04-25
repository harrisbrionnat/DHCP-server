<p align="center">
  <img src="https://imgur.com/RDJaX9P.png" alt="Microsoft Active Directory Logo"/>
</p>

# How to Install and Configure a DHCP Server on Windows Server 2022 

This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.

## Environments and Technologies Used
- Microsoft Azure (Virtual Machines)
- Remote Desktop
  

## Operating Systems Used
- Windows Server 2022
- Windows 10 (21H2)

## High-Level Installation and Configuration Steps
- Configure a static IP address for the server


## Deployment and Configuration Steps

1. go to the Start menu. Type in view network connections. Right click on the interface and select 'Properties'. Select IPv4 and then click 'Properties'. Fill in a static ip address. Along with the subnet mask and Default Gateway. Also assign a static IP for the DNS Server. Then restart the system. I just used the IP addresses inherited from my Azure virtual machine.
   
2. Click on Manage. Click on 'add roles and features' Select next on the first screen. On the next screen select 'role based or feature based installation'. Select the correct server. Select DHCP server. Make sure the box to include management tools is selected. Then click 'Add Features'. Keep clicking next until you get to the 'Confirmation' Tab. Make sure the box is checked to restart the destination server is checked and install.
3. Click the yellow caution sign and click 'complete DHCP configuration'. On the box that pops up, click 'commit'. Then, restart the computer.
   
   <p align="center">
     <img src="https://imgur.com/1hM7PZM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
   </p>
5. Now, we can configure DHCP settings. On the Server Manager, go to 'Tools' and then 'DHCP'. Click on the server name and then right click on IPv4. Select 'new scope'. Select next' on the wizard. Add a scope name and description. Type in a starting IP and an ending ip ip for your scope. The add in the subnet mask. Click 'next'. If you want to exclude an IP from that range, specify on the screen. I have not, as my DHCP server and default Gateway ip does not fall withing the DHCP scope created. Set the lease duration. I kept the default--which is 8. Keep clicking next until you get to the active scope screen. Then click 'Yes, I want to active now'.
    <p align="center">
     <img src="https://imgur.com/XNmmNKs.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
   </p>
   <p align="center">
     <img src="https://imgur.com/UPkX3Cr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
   </p>
   <p align="center">
     <img src="https://imgur.com/t3Dhxf0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
   </p>
    
7. Set the Domain Controller's NIC's private IP address to static. Go to the Virtual Machines tab, click on `active-directory-dc`, then go to **Networking** → **Network Settings**. Click on the virtual NIC, then on **ip-config1**. Change the private IP setting from dynamic to static.
8. For testing purposes, disable the Windows Firewall on `active-directory-dc`. Once logged in to the VM, right-click the start menu and select **Run**. Type `wf.msc`, click **Windows Defender Firewall Properties**, and turn off the firewall for the Domain, Private, and Public Profile tabs.
   <p align="center">
     <img src="https://imgur.com/l2geFfX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
   </p>
9. Set `active-directory-client`'s DNS server to `active-directory-dc`'s private IP address. On `active-directory-client`, go to **Networking** → **Network Settings**. Click on the virtual NIC, then go to **DNS Servers**, choose 'Custom', and enter `active-directory-dc`'s private IP. Log into `active-directory-client` and ping `active-directory-dc`'s private IP address. Restart `active-directory-client` from the Azure Portal. You can run `ipconfig /all` to check if the DNS settings reflect the domain controller's private IP.
   <p align="center">
     <img src="https://imgur.com/8DriULG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
   </p>
10. Install Active Directory Domain Services on `active-directory-dc`. Once logged into the VM, go to **Server Manager**, then **Add Roles and Features**. Click next until you reach **Select Server Roles**. Check **Active Directory Domain Services** and then click **Install**.
   <p align="center">
     <img src="https://imgur.com/CicuXFZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
   </p>
11. Promote `active-directory-dc` to a domain controller. In Server Manager, click the flag in the upper right corner. Select **Promote this server to a domain controller**, create a new forest with the name `sampledomain.com`, and create a password. Click through the prompts and install. The computer should automatically restart.
   <p align="center">
     <img src="https://imgur.com/JqtkKVX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
   </p>

11. Create a domain admin user within the domain controller using the newly created domain user credentials. Your domain login should look like: `DOMAIN\user` followed by your password. To create a domain admin account:
   - Open **Active Directory Users and Computers**.
   - Right-click `mydomain.com` and add two **Organizational Units**: `_EMPLOYEES` and `_ADMINS`.
   - Create a user to put in the `_ADMINS` organizational unit. Right-click the OU and select **New** → **User**.
   - Fill in the user's first name, last name, and domain account name.
   - Add this user to the Domain Admins Security group by right-clicking **Properties** → **Member Of** → **Add**. Enter `Domain Admins`, click **Apply**, then **OK**. Log out and log in as the newly created domain admin.
   - Create four users to put in the `_EMPLOYEES` organizational unit. Right-click the OU and select **New** → **User**.
   - Fill in the user's first name last name, and domain account name.
   <p align="center">
     <img src="https://imgur.com/QNjMygk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
   </p>
   <p align="center">
     <img src="https://imgur.com/F7Gta4p.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
   </p>

11. Join `active-directory-client` to the domain. Log in to that PC and go to **Start** → **System** → **Rename this PC (Advanced)** → **Computer Name**. Join the domain `mydomain.com`. Verify that `active-directory-client` is part of the domain by logging into the domain controller and navigating to **Active Directory Users and Computers** → `mydomain.com` → **Computers**.
    <p align="center">
      <img src="https://imgur.com/LgSVIrH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
    </p>

12. Login to `active-directory-client` as the domain admin user. Go to **System** → **Remote Desktop** → **User Accounts** → **Select Users that Can Remotely Access this PC**, and add **Domain Users**.
    <p align="center">
      <img src="https://imgur.com/PR03uDI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
    </p>
