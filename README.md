<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-Premises Active Directory Deployed in the Cloud (Azure)</h1>
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

<h3>Ensure Connectivity between the Client-1 and DC-1 (Domain Controller)</h3>
<p>
  Attempt to ping DC-1’s private IP address 
  
● Ensure the ping succeeded 

Open PowerShell ISE and ping DC-1 Private IP address
</p>
<p>
  <img src="https://github.com/user-attachments/assets/97f3dd37-df09-4fab-9d30-2a3b493616f9" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  From Client-1, open PowerShell and run ipconfig /all 
  
● The output for the DNS settings should show DC-1’s private IP Address
</p>
<p>
<img src="https://github.com/user-attachments/assets/90dbd91f-054d-4665-89a0-78ba49fd206a" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Now Client-1 DNS Servers should show DC-1 Private IP Address.
</p>
<br />

<h3>Install Active Directory</h3>
<p>
  Login to DC-1 and Install Active Directory Domain Services 
</p>
<p>
  <img src="https://github.com/user-attachments/assets/21d4672d-5f52-41cd-a4ca-744e6bf5796b" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  <img src="https://github.com/user-attachments/assets/c5ef2b12-351a-44b1-af30-b4468fab680b" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://github.com/user-attachments/assets/947620f1-d4fe-4c61-ad92-fb113b6f1b6a" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Now Promote as a DC (Domain Controller): Setup a new forest as mydomain.com 
  
  (can be anything, just remember what it is) 
</p>
<p>
  <img src="https://github.com/user-attachments/assets/c23e8d69-82aa-45f9-bbc3-2bce87cf30df" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Click “Add a new Forest”, and name it “mydomain.com”
</p>
<p>
  <img src="https://github.com/user-attachments/assets/33e416ba-7830-4786-8f82-1f6559223fe5" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Restart and then log back into DC-1 as user: mydomain.com\labuser
</p>
<p>
  <img src="https://github.com/user-attachments/assets/18d08719-69e2-4e64-935a-a60694c540c7" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<h3>Create a Domain Admin User within the Domain</h3>
