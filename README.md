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


Prior to this tutorial, you must have created a  Windows Server 2020 Virtual Machine and Windows 10 or 11 Virtual Machine and put them in the same network.



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
    
7. Log into the client machine operating Windows 10. Go to start and view network connections. Right click on the interface and select 'Properties' Select Ipv4 and then 'Properties' Make sure your computer is set to obtain ip addresses automatically.
   
   



