<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-Premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>💻⚙️ Software and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- **Windows Server (2022)**
- **Windows 10 Pro (22H2)**
- Remote Desktop
- **Windows Server Manager**
- **Active Directory Domain Services (ADDS)**
- **Active Directory Users and Computers (ADUC)**
- **PowerShell**

<h2>💻 Operating Systems Used </h2>

- **Windows Server (2022)**
- **Windows 10 Pro (22H2)**

<h2>High-Level Deployment and Configuration Steps</h2>

- Create Resource Group
- Create Virtual Network
- Set up the Domain Controller (Windows Server) VM
- Set up the Client (Windows 10 Pro) VM
- Ensure Connectivity between Client-1 and Domain Controller
- Install and Configure Active Directory
- Create a Domain Admin Account
- Join Client-1 to the Domain Controller (mydomain.com)
- Set up Remote Desktop for non-administrative users on Client-1
- Create 2000 users & attempt to log into Client-1 with one of the users

<h2>What is Active Directory?</h2>
<p>
  <img src="https://i.imgur.com/gLlT8B8.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<h3>🔵 Active Directory Components</h3>
<p>
  
  **🔹️ Key Components of AD**
  
  - **Domain** - Logical group of objects
  - **Domain Controller** - Authenticates Logins
  - **Forest** - Collection of Domains 
  - **OU** - Organize users & computers
  - **Group Policy** - Apply rules & settings
  
</p>
<h3>Familiar Use</h3>
<p>
  <img src="https://i.imgur.com/SHwtjAg.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<h3>What we'll do - (Active Directory Lab)</h3>
<p>
  In this Active Directory Lab, we'll create 2 VMs in the Azure Cloud and on the same VNET (Virtual Network). One will be the DC (Domain Controller), and the other will be a Client machine. We will change the DC to a Static IP address because it will offer Active Directory Services to the Client's machine.
  
  For that, the Client machine will have to join the Domain. We then have to change the DNS settings on the Client machine so that the Client machine will use the DC (Domain Controller) as its DNS Server.
</p>
<p>
  <img src="https://i.imgur.com/SuvHoB1.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>
</p>
<h3>Active Directory Setup explained</h3>
<p>
  
  <h4>🧩 How They Work Together:</h4>
  
  🔹️ Component - Role in the Setup
  - **Windows Server** - Host OS that runs all other components.
  - **DNS Server** - Helps clients find services like AD.
  - **Domain** - A Logical group of networked computers that share a central directory database and are managed by a DC.
  - **Forest** - the highest-level container in Active Directory. It can contain one or more domains that trust each other.
  - **Domain Controller** - Manages the AD database and authenticates users.

<h4>🔷️ To summarize,</h4>

In an Active Directory home lab, Windows Server serves as the base operating system for hosting services. When you install the Active Directory Domain Services (ADDS) role, the server becomes a Domain Controller (DC), managing a Domain — a centralized environment for user and device authentication. The first domain created also becomes the Forest root, forming the top-level container for all domains. 

As part of this setup, a DNS Server is typically installed to help clients locate domain resources like the DC itself. The DNS Server is essential for AD to resolve names and services within the domain.

Together, these components enable a functional AD environment where devices can join the domain and authenticate through the DC using DNS resolution.
  
</p>
<br />

<h2>High-Level Deployment and Configuration Steps</h2>

<h3>🔵 Setup Azure Resources</h3>
<p>
  We'll be using Microsoft Azure, so log in to your Azure Account.
</p>
<p>
  <img src="https://i.imgur.com/KS5Ad2k.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a Resource Group 
</p>
<p>
<img src="https://i.imgur.com/sweX6x4.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a Virtual Network and Subnet
</p>
<p>
  <img src="https://github.com/user-attachments/assets/a4259798-ae22-427d-a70d-c4b1fa95d28a" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

  We will log in to the 2 VMs through the "**Remote Desktop Connection**" app on your PC using the VMs' Public IP Addresses. 
</p>
<p>
  <img src="https://i.imgur.com/UFdri8J.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>🔷️ Create the Domain Controller (Windows Server 2022) VM in Azure named “DC-1”</h3>
<p>
  
  Ensure that the VM is in the same Region as the Virtual Network and that the VM is part of the same Virtual Network that was created.
  
  ● Username: **labuser**
  
  ● Password: **Cyberlab123!**
</p>
<p>
<img src="https://i.imgur.com/xwzAG8x.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
 <img src="https://i.imgur.com/dgniaoK.png" height="100%" width="100%" alt="Disk Sanitization Steps"/> 
</p>
<p>
  
After creating the VM, set the domain controller’s NIC private IP address to "**Static**", meaning the **Private IP address won’t change**.

Here's trivia on Static IP Addresses, 

- **Static IP addresses** provide consistent network identification, which is crucial for servers, network devices, and services that need to be reliably accessible.
  
- They simplify DNS management, as the IP address does not change, reducing the risk of connectivity issues. Static IPs are also essential for remote access, hosting websites, and running email servers, as they ensure that external devices can consistently connect to the correct address.

- However, they require manual configuration and management, which can be more time-consuming compared to **Dynamic IP addresses**.

🔹️ Go to DC-1's **Networking tab -> Network Settings -> ipconfig1(primary) -> ipconfig1 -> Edit IP configuartion**
  
This is because this server will act as a DNS Server.
</p>
<p>
  <img src="https://i.imgur.com/eXr0xTF.png" height="100%" width="90%" alt="Disk Sanitization Steps"/> 
</p>
<br />

<p>
  
  Log in to the DC-1 VM and **Disable** the Windows Firewall. 
  
  Since this is a Server, it must **accept Inbound Traffic❕️**. 
  
  (For Testing Connectivity with Ping)
</p>
<p>
<img src="https://i.imgur.com/YaFNaGN.png" height="80%" width="90%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/0uCdkF7.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  <h4>Troubleshooting❗</h4>
  
  ◻️ In case there's trouble when Pinging DC-1 from Client-1 in PowerShell in the later steps, go back into the 

  Firewall Settings (in DC-1 VM) and

  - Click On "**Windows Defender Firewall Properties**"

  - Turn Off all of the Firewall states, EXCEPT "**Public Profile**",

  - Under the Public Profile Tab, go to "**INBOUND CONNECTIONS**" and click "**ALLOW**",

  This helped when I had trouble pinging DC-1 from Client-1.
</p>
<br />

<h3>🔷️ Setup "Client-1" (Windows 10 Pro) VM in Azure</h3>
<p>
Create the Client VM (Windows 10 Pro) named “Client-1” 
  
  Again, attach it to the same Region and Virtual Network as "DC-1"

  Give it the same Username & Password as DC-1, for convenience,
  
  ● Username: **labuser** 
  
  ● Password: **Cyberlab123!**
</p>
<p>
  <img src="https://i.imgur.com/NVRLbrO.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Ensure that both VMs are in the same Vnet (you can check the Topology with Network Settings),
</p>
<p>
  <img src="https://i.imgur.com/Kw5H63Z.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  After VM is created, Set Client-1’s DNS settings to DC-1’s Private IP address.
</p>
<p>
  <img src="https://github.com/user-attachments/assets/5ced891b-37c2-4251-8ca3-61ba64b1f6d8" height="90%" width="90%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
  Copy Private IP Address from DC-1 VM
  
  🔹️ In the Azure portal, navigate to Client-1 VM, 
  
  **Networking -> Network Settings -> Client-1's Network Interface/IP Configuration -> DNS servers -> Custom**, and paste the DC-1's **Private IP Address**, then click save. 
  
  Doing this allows Client-1 to join the DC-1's **DNS Server**.
</p>
<p>
  <img src="https://i.imgur.com/xpUQSkV.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

  <h4>Troubleshooting ❗️⚠️</h4>
  
  During the lab, I had trouble saving Client-1's DNS settings to point to DC-1's Private IP Address. Here's what to do if this error appears in Azure VMs.

  <img src="https://i.imgur.com/2DaMFi8.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>

  To resolve this issue, verify the Network Security Group (NSG) Rules: Ensure that any Network Security Group (NSG) rules permit traffic on Port 53 (DNS) from Client-1 to DC-1. 
  
  If traffic is restricted, Client-1 won’t be able to resolve the domain through DC-1.

  ◻️ To fix this, do the following steps:
  
1. Search for "Network security groups" in Azure and click into "Network security groups" when it comes up (Not the classic version).

2. Click into "client-1-nsg" (or whatever it's named) and do the following steps:

    - Click the "Settings" dropdown menu

    - Click "**Inbound security rules**"

    - Click "Add"

    - Leave everything alone except in the "**Destination port ranges**" field; change the number to **53**, and in the "**Priority**" field, change it to **290**.

    - Click the "Save" button

    - Then click "**Outbound security rules**" and complete the same process again,

3. Then click into "dc-1-nsg" (for DC-1's VM) and do the same process again.

4. Go to "Virtual Machines"

5. Click the checkbox next to "Client-1" and "DC-1" and click "Restart" in the top bar,

  (*This helped me resolve the issue. After resolving the issue, do the process of pasting DC-1's Private IP Address to Client-1's DNS servers, and restart Client-1*.)

  Also, restart DC-1 again just in case.
</p>
<br />

<h3>🌐 Ensure Connectivity between the Client-1 and DC-1 (Domain Controller)</h3>
<p>
  From Client-1, attempt to ping DC-1’s Private IP address 
  
● Ensure the Ping succeeded 

Open PowerShell as Administrator and Ping the DC-1 Private IP address
</p>
<p>
  <img src="https://github.com/user-attachments/assets/97f3dd37-df09-4fab-9d30-2a3b493616f9" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
  From Client-1, open PowerShell as Admin and run `ipconfig /all` 
  
● The output for the Client-1's DNS settings should show DC-1’s Private IP Address
</p>
<p>
<img src="https://github.com/user-attachments/assets/90dbd91f-054d-4665-89a0-78ba49fd206a" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Now Client-1's DNS Servers should show DC-1's Private IP Address.
</p>
<br />

<h3>🌐 Install and Configure Active Directory</h3>
<p>
  
  Log in to DC-1, go to 🔹️ **Server Manager**, 

Click "**Add Roles and Features**" on the Dashboard, on "**Server Selection**" check DC-1, and install **Active Directory Domain Services (ADDS)**. 
  
  At "Confirmation", check✅️ restart if required. 
</p>
<p>
  <img src="https://github.com/user-attachments/assets/21d4672d-5f52-41cd-a4ca-744e6bf5796b" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  <img src="https://github.com/user-attachments/assets/c5ef2b12-351a-44b1-af30-b4468fab680b" height="100%" width="100%" alt="Disk Sanitization Steps"/>
  <img src="https://github.com/user-attachments/assets/947620f1-d4fe-4c61-ad92-fb113b6f1b6a" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
  Click the **(*Flag* 🏳⚠️)** on the top right, and Promote DC-1 as a **Domain Controller (DC)**. 
  
  Set up a new forest as "mydomain.com".
  
  (Can be named anything, just remember what it is) 
</p>
<p>
  <img src="https://github.com/user-attachments/assets/c23e8d69-82aa-45f9-bbc3-2bce87cf30df" height="70%" width="90%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
  Click “**Add a new Forest**”, and name it "mydomain.com",
</p>
<p>
  <img src="https://github.com/user-attachments/assets/33e416ba-7830-4786-8f82-1f6559223fe5" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
  Restart and then log back into DC-1 as user: **"mydomain.com\labuser"**
</p>
<p>
  <img src="https://github.com/user-attachments/assets/18d08719-69e2-4e64-935a-a60694c540c7" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>👨‍💻 Create a Domain Admin User within the Domain</h3>
<p>

  In an Active Directory (AD) environment, the **Domain Admin user(s)** are a highly privileged account that has full control over the domain and all of its resources.

  ✅ What Is the **"Domain Admin User"?**
  
  - The Domain Admin user is a member of the "Domain Admins" security group in Active Directory.
  - This group is part of the Administrators group on all domain-joined computers by default.
  - **Members of the Domain Admins group have full administrative privileges throughout the domain**.
  - Full control over the domain and domain-joined machines.
<br />
  🧑‍💼 Default Domain Admin User
  
  - When you first promote a server to be a Domain Controller (using dcpromo or equivalent), a default user is created: Username: Administrator.
  - Here, I used the original local admin (labuser) to create a Domain Admin User, which will be "Jane Doe".  
<br />
  🔐 Permissions of a Domain Admin User

   Domain Admin users can:
  - Create, modify, and delete user accounts and groups.
  - Create and manage Group Policy Objects (GPOs) and Organizational Units (OUs).
  - Access and configure any domain-joined computer(s).
  - Promote or Demote Domain Controllers.
  - Control access to files, folders, printers, and other network resources.
  - Delegate administrative tasks to others.

  **Let's Begin,**
  
  In **Active Directory Users and Computers (ADUC)**, create an Organizational Unit (OU) called “_EMPLOYEES”, 
</p>
<p>
  <img src="https://github.com/user-attachments/assets/90e9462a-7a88-4f37-8b21-8f5e059b5140" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Then create a new OU named “_ADMINS”, 
</p>
<p>
  <img src="https://github.com/user-attachments/assets/7e615a7e-6009-4fd2-b61f-3604da82d5ab" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Create a new employee named “Jane Doe” (same password) with the username of “jane_admin” / Cyberlab123! 
</p>
<p>
  <img src="https://github.com/user-attachments/assets/d37838c0-fd89-48ae-9162-f7f606a11e78" height="100%" width="100%" alt="Disk Sanitization Steps"/>
  <img src="https://github.com/user-attachments/assets/40311315-9b81-4edf-a531-c8128cb654ee" height="70%" width="90%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Add "jane_admin" to the “Domain Admins” Security Group 
</p>
<p>
  <img src="https://github.com/user-attachments/assets/97669e85-5cd0-41c8-b3d3-8e09a4d7143d" height="100%" width="100%" alt="Disk Sanitization Steps"/>
  <img src="https://github.com/user-attachments/assets/c2159fd6-bc8a-4a65-b571-cfe0faf40153" height="70%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
  Log out / close the connection to DC-1 and log back in as 
  
  **“mydomain.com\jane_admin”** 

  User **"jane_admin"** will be the Admin Account from now on.
</p>
<p>
  <img src="https://github.com/user-attachments/assets/9e634d11-1200-46bc-ab55-d424ec3a8c29" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>🟢 Join Client-1 to your Domain (mydomain.com)</h3>
<p>
  
  Log in to Client-1 as the original local admin (**labuser**) and join it to the Domain.

  🔹️ Go to **System -> About -> Rename this PC(advanced) -> System Properties -> Computer Name -> Change -> Domain**

  Then log in as the Domain Admin with the Login credentials and password,
  
(Computer / VM will restart) 
</p>
<p>
  <img src="https://i.imgur.com/y3MUjQ2.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/mPpcVUd.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/1eBxFN9.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
  Log in to the DC-1 / Domain Controller and verify Client-1 PC shows up in **ADUC**.
</p>
<p>
  <img src="https://i.imgur.com/ndAcxiW.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Create a new OU named “_CLIENTS” and drag Client-1 into there
</p>
<p>
  <img src="https://i.imgur.com/OHOiZgq.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>🖥 Setup Remote Desktop for non-administrative users on Client-1 </h3>
<p>
  
  Log into Client-1 as **"mydomain.com\jane_admin"** - (It takes time for a Domain User/Admin to log into the PC for the 1st time) 
</p>
<p>
  <img src="https://i.imgur.com/RG0OK0o.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
  🔹️ Open System Properties by going to **Settings -> System -> About ->**
  
  Click **“Remote Desktop”**, 
  
  Select **"User Accounts"** and click **Add**.

  Type/Allow **“Domain Users”** access to the remote desktop and click ok. 
  
  This will allow all Domain Users (non-administrative users) by default to log into the Client-1 PC.
</p>
<p>
  <img src="https://i.imgur.com/irf2ryg.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  You can now log into Client-1 as a normal, non-administrative user. 
</p>
<br />

<h3>🔵 Create 2000 additional users & attempt to log into Client-1 with one of the users</h3>
<p>
  
  Lastly, let's verify that normal users can use RDP (Remote Desktop) to log into Client-1. We will use a script to generate **2000 normal users** into the Domain.

  The script will be input into PowerShell ISE as Administrator. After the users are created, select a random user and use RDP (Remote Desktop) to log into Client-1.
  
  Login to DC-1 as "jane_admin", 
  
  Open **PowerShell ISE** as an **Administrator**,

  Create a new File and paste the contents of the script into it.

  **🔹️ Script link**: https://github.com/Xinloiazn/configure-ad/blob/main/adscript.ps1

  Run the script and observe the accounts being created,
</p>
<p>
  <img src="https://i.imgur.com/ZPbcX40.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/CUNZvwM.png" height="70%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  When finished, open ADUC and observe the 2000 accounts in the appropriate OU (Organizational Unit), which was the "_EMPLOYEES" OU file.　
  
  ("_EMPLOYEES") - (Make sure the OU is spelled correctly since this particular script will search for the correct OU file in ADUC)
</p>
<p>
   <img src="https://i.imgur.com/7UaNiRC.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
  Now, attempt to log into Client-1 with one of the user accounts we created with the script,

  - *User*: **"BOB.LIJ"**
  
  (Take note of the password in the script!)
</p>
<p>
  <img src="https://i.imgur.com/uJldo10.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/3fyoSeK.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/xpJO6bx.png" height="70%" width="100%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/WRyd7pe.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>Conclusion</h3>
<p>

For this Active Directory lab, we completed the Active Directory Deployment and Configuration phase. By configuring Active Directory on the Domain Controller, we established our infrastructure by creating a **Forest**, a **Domain Administrator account**, and **joining Client-1 PC to the Domain**.
  
  This tutorial was to help us have a better understanding of Active Directory, Domain environments, GPOs, along with
  Understanding network security protocols through VMs within Azure's Cloud.

  When finished, close the Remote Desktop connection, delete the Resource Group(s) we created at the beginning of 
  this tutorial, and verify the Resource Group deletion to avoid unnecessary charges.
</p>
