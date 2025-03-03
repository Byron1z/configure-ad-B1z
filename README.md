<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Software and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

<p>
  Create a Resource Group 
</p>
<p>
<img src="https://i.imgur.com/sweX6x4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a Virtual Network and Subnet
</p>
<p>
  <img src="https://github.com/user-attachments/assets/a4259798-ae22-427d-a70d-c4b1fa95d28a" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
  Create the Domain Controller VM (Windows Server 2022) named “DC-1” 
  
  Make sure to create the VM in the Same Region as your Virtual Network and make sure the VM is in the Virtual Network we made.
  
  ● Username: labuser 
  
  ● Password: Cyberlab123!
</p>
<p>
<img src="https://i.imgur.com/OZn32ZK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 <img src="https://github.com/user-attachments/assets/d411910a-f38b-48cd-b284-80d4b606d791" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
</p>
<p>
After VM is created, set the Domain Controller’s NIC Private IP address to be static, meaning the Private IP Address won’t change. 
  
  This is because this server will act as a DNS server.
</p>
<p>
  <img src="https://i.imgur.com/eXr0xTF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
</p>
<br />

<p>
  Log into DC-1 VM and disable the Windows Firewall (for Testing Connectivity)
</p>
<p>
<img src="https://i.imgur.com/0uCdkF7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3> Setup Client-1 in Azure </h3>
<p>
Create the Client VM (Windows 10 Pro) named “Client-1” 
  
  Again, Attach it to the same Region and Virtual Network as "DC-1"

  Give it the same Username & Password as DC-1, for convenience
  
  ● Username: labuser 
  
  ● Password: Cyberlab123!
</p>
<p>
  <img src="https://i.imgur.com/9Tt7JiZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  After VM is created, set Client-1’s DNS settings to DC-1’s Private IP address.
</p>
<p>
  <img src="https://github.com/user-attachments/assets/5ced891b-37c2-4251-8ca3-61ba64b1f6d8" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Copy Private IP Address from DC-1 VM
  
  Go to Client-1, Networking -> Network Settings -> Network Interface/IP Configuration -> DNS servers -> Custom and paste to DC-1's Private IP Address. 
  
  Doing this allows Client-1 to join the Domain.
</p>
<p>
  <img src="https://i.imgur.com/xpUQSkV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  From the Azure Portal, restart Client-1.
</p>
<br />
