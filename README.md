<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-Premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Software and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Windows Server (2022)
- Windows 10 Pro (22H2)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server (2022)
- Windows 10 Pro (22H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create Resource Group
- Create Virtual Network
- Ensure Connectivity between the Client and Domain Controller
- Install Active Directory
- Create an Domain Admin Account
- Join Client-1 to the Domain Controller (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create additional users and attempt to log into Client-1 with one of the users

<h2>What is Active Directory?</h2>
<p>
  <img src="https://i.imgur.com/gLlT8B8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<h3>Familiar Use</h3>
<p>
  <img src="https://i.imgur.com/SHwtjAg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<h3>What we'll do - (Active Directory Lab)</h3>
<p>
  In this Active Directory Lab, we'll create 2 VMs in Azure and on the same VNET (Virtual Network). One will be a DC (Domain Controller), and the other will be a Client machine. We will change the DC to a static IP address because it will offer Active Directory Services to the Client's machine. For that, the Client machine will have to join the Domain. We then have to change the DNS settings on the Client machine so that the Client machine will use the DC (Domain Controller) as its DNS Server.
</p>
<p>
  <img src="https://i.imgur.com/SuvHoB1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h2>Deployment and Configuration Steps</h2>

<h3>Setup Azure Resources</h3>
<p>
  We'll be using Microsoft Azure, so log into your Azure Account
</p>
<p>
  <img src="https://i.imgur.com/KS5Ad2k.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
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
<p>
  We will log into the 2 VM's through the Remote Desktop app on your PC using the VMs' Public IP Addresses. 
</p>
<p>
  <img src="https://i.imgur.com/UFdri8J.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>Create the Domain Controller VM (Windows Server 2022) named “DC-1” </h3>
<p> 
  Make sure to create the VM in the Same Region as your Virtual Network and make sure the VM is in the Same Virtual Network we made.
  
  ● Username: labuser 
  
  ● Password: Cyberlab123!
</p>
<p>
<img src="https://i.imgur.com/xwzAG8x.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 <img src="https://i.imgur.com/dgniaoK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
</p>
<p>
After creating the VM, set the domain controller’s NIC private IP address to static, meaning the private IP address won’t change. 
  
  This is because this server will act as a DNS Server.
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
<p>
  Edit: In case if there's trouble when pinging DC-1 from Client-1 in PowerShell in the later steps, go back into the 

  Firewall Settings (in DC-1 vm) and

  - Clicked On "Windows Defender Firewall Properties"

  - Turn Off all of the firewall states, EXCEPT "Public Profile".

  - Under the Public Profile Tab, go to "INBOUND CONNECTIONS" and clicked "ALLOW"

  This helped when I had trouble pinging DC-1 from Client-1.
</p>
<br />

<h3> Setup "Client-1" (Windows 10 Pro) VM in Azure </h3>
<p>
Create the Client VM (Windows 10 Pro) named “Client-1” 
  
  Again, Attach it to the same Region and Virtual Network as "DC-1"

  Give it the same Username & Password as DC-1, for convenience
  
  ● Username: labuser 
  
  ● Password: Cyberlab123!
</p>
<p>
  <img src="https://i.imgur.com/NVRLbrO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Ensure that both VMs are in the same Vnet (you can check the Topology with Network Settings)
</p>
<p>
  <img src="https://i.imgur.com/Kw5H63Z.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  After VM is created, Set Client-1’s DNS settings to DC-1’s Private IP address.
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
  From the Azure Portal, Restart Client-1.
</p>
<br />

<h3>Ensure Connectivity between the Client-1 and DC-1 (Domain Controller)</h3>
<p>
  Attempt to ping DC-1’s Private IP address 
  
● Ensure the Ping succeeded 

Open PowerShell ISE and Ping the DC-1 Private IP address
</p>
<p>
  <img src="https://github.com/user-attachments/assets/97f3dd37-df09-4fab-9d30-2a3b493616f9" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  From Client-1, open PowerShell and run "ipconfig /all" 
  
● The output for the Client-1's DNS settings should show DC-1’s Private IP Address
</p>
<p>
<img src="https://github.com/user-attachments/assets/90dbd91f-054d-4665-89a0-78ba49fd206a" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Now Client-1's DNS Servers should show DC-1 Private IP Address.
</p>
<br />

<h3>Install Active Directory</h3>
<p>
  Login to DC-1, go to Server Manager and Install Active Directory Domain Services 
</p>
<p>
  <img src="https://github.com/user-attachments/assets/21d4672d-5f52-41cd-a4ca-744e6bf5796b" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  <img src="https://github.com/user-attachments/assets/c5ef2b12-351a-44b1-af30-b4468fab680b" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://github.com/user-attachments/assets/947620f1-d4fe-4c61-ad92-fb113b6f1b6a" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Now Promote DC-1 as a DC (Domain Controller): Setup a new forest as "mydomain.com"
  
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
<p>
  In Active Directory Users and Computers (ADUC), create an Organizational Unit 
(OU) called “_EMPLOYEES” 
</p>
<p>
  <img src="https://github.com/user-attachments/assets/90e9462a-7a88-4f37-8b21-8f5e059b5140" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Create a new OU named “_ADMINS” 
</p>
<p>
  <img src="https://github.com/user-attachments/assets/7e615a7e-6009-4fd2-b61f-3604da82d5ab" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Create a new employee named “Jane Doe” (same password) with the username of “jane_admin” / Cyberlab123! 
</p>
<p>
  <img src="https://github.com/user-attachments/assets/d37838c0-fd89-48ae-9162-f7f606a11e78" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://github.com/user-attachments/assets/40311315-9b81-4edf-a531-c8128cb654ee" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Add jane_admin to the “Domain Admins” Security Group 
</p>
<p>
  <img src="https://github.com/user-attachments/assets/97669e85-5cd0-41c8-b3d3-8e09a4d7143d" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://github.com/user-attachments/assets/c2159fd6-bc8a-4a65-b571-cfe0faf40153" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Log out / close the connection to DC-1 and log back in as 
  
“mydomain.com\jane_admin” 

User jane_admin will be the Admin Account from now on
</p>
<p>
  <img src="https://github.com/user-attachments/assets/9e634d11-1200-46bc-ab55-d424ec3a8c29" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<h3>Join Client-1 to your domain (mydomain.com)</h3>
<p>
  Login to Client-1 as the original local admin (labuser) and join it to the domain 

  Go to System -> About -> Rename this PC(advanced) -> Computer Name -> Change -> Domain
  
(computer will restart) 
</p>
<p>
  <img src="https://i.imgur.com/y3MUjQ2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/mPpcVUd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/1eBxFN9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Login to the Domain Controller and Verify Client-1 shows up in ADUC 
</p>
<p>
  <img src="https://i.imgur.com/ndAcxiW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Create a new OU named “_CLIENTS” and drag Client-1 into there
</p>
<p>
  <img src="https://i.imgur.com/OHOiZgq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<h3>Setup Remote Desktop for non-administrative users on Client-1 </h3>
<p>
  Log into Client-1 as mydomain.com\jane_admin 
</p>
<p>
  <img src="https://i.imgur.com/RG0OK0o.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Open system properties 
  
Click “Remote Desktop” 

Allow “domain users” access to remote desktop 
</p>
<p>
  <img src="https://i.imgur.com/irf2ryg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  You can now log into Client-1 as a normal, non-administrative user now 
</p>
<br />
<h3>Now Create a bunch of additional users and attempt to log into Client-1 with one of the users</h3>
<p>
  Login to DC-1 as jane_admin 
  
Open PowerShell ISE as an Administrator 

Create a new File and paste the contents of the script into it 

Script link: https://github.com/Xinloiazn/configure-ad/blob/main/adscript.ps1

Run the script and observe the accounts being created
</p>
<p>
  <img src="https://i.imgur.com/ZPbcX40.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/CUNZvwM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  When finished, open ADUC and observe the accounts in the appropriate OU　
  
(_EMPLOYEES)
</p>
<p>
   <img src="https://i.imgur.com/7UaNiRC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Now attempt to log into Client-1 with one of the user accounts we created with the script 
  
  (take note of the password in the script)
</p>
<p>
  <img src="https://i.imgur.com/uJldo10.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/3fyoSeK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/xpJO6bx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/WRyd7pe.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<p>
  This tutorial was to help us have a better understanding Active Directory and Domain Controllers, along with
  understanding network security protocols through VM's within the Cloud.

  When finished, Close the Remote Desktop connection, delete the Resource Group(s) we created at the beginning of 
  this tutorial, and verify the Resource Group deletion to avoid unnecessary charges.
</p>
