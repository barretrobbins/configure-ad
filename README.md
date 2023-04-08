<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.<br />

<h2>Video Demonstration</h2>

- ### [YouTube: How to Setup Active Directory using Microsoft Azure.](https://www.youtube.com/watch?v=CHO33aQHrXU)


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Powershell

<h2>Operating Systems Used</h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1 - Setup Azure Domain Controller (windows server) and Azure Client (Windows 10) VMs.
- Step 2 - Install Active Directory in the Domain Controller.
- Step 3 - Join Client-1 to the Active Directory Domain.
- Step 4 - Setup remote desktop login for Admin and non-Admin users/ create user accounts.

<h2>Deployment and Configuration Steps</h2>

1 - Setup Azure Domain Controller (windows server) and Azure Client (Windows 10) VMs.
<p>
<img src="https://i.imgur.com/b1zTN0s.png" height="80%" width="80%" alt="DC-1 setup"/>
</p>
<p>
Create a Virtual Machine for the Domain Controler.  First, name the Resource Group and then select a region for the Domain Controler or DC-1; the region is based on the region closest to your location.  Second, select the image, for example: Windows 22 Datacenter Server.  Third, select the size: Standard_E2s_v3-2vcpus, 16 Gib memory, and then scroll to the bottom. Click on Disks->Networking-> Review + create-> and finally Create Virtual Machine DC-1.  

Next, create the second Virtual Machine for Client-1.  The region should be the same as the area selected for the Domain Controler (DC-1).  For this VM, the image is Windows 10 Pro, version 21H2-64 Gen2.  The size will be the same as the first virtual machine (DC-1).  Scroll to the bottom, click on Disks
-> Networking-> Review + create, and then create Virtual Machine Client-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/2XVxIgi.png" height="80%" width="80%" alt="DC-1 with a static private IP address"/>
</p>
<p>
The following step is to change the NIC Private IP address for DC-1 to static instead of dynamic. Go to DC-1, click the Networking tab, click the Network Interface number, click the IP Configurations tab, click the Private IP address, and then change the assignment from dynamic to static.
</p>
<br />

2 - Install Active Directory in the Domain Controller.
<p>
<img src="https://i.imgur.com/YFOxsr6.png" width="80%" alt="Active Directory"/>
</p>
<p>
Next step is to install the Active Directory on DC-1.  To install the Active Directory, click on "add roles and features" in Server Manager and then click "next" on the bottom.  Click on Server Selection on the left side, which will automatically take you to Server Roles; then select Active Direcotry Domain Services.  Click "add features", then click next, next, and next again.  Install "Active Directory Domain Services".
</p>
<br />

<p>
<img src="https://i.imgur.com/SZFfluD.png" height="80%" width="80%" alt="Creating a domain"/>
</p>
<p>
After installing Active Directory, go to the top right corner and click on the yellow exclamation mark sign.  Click "Promote this server to a domain controller", click "add a new forest", and then create a "Root Domain Name".  After DC-1 restarts, log back in using your domain name and login user name. Example: "mydomain.com\labuser".
</p>
<br />

<p>
<img src="https://i.imgur.com/LnJBSWa.png" height="50%" width="50%" alt="new employee creation"/>
</p>
<p>
Once the Active Directory installation has been completed, the next step is to create a some users. In the Search bar next to the Windows icon, type and go to "Active Directory Users and Computers", and create an Organizational Unit named "_EMPLOYEES". Inside the new folder, create an employee with your name and username.  Add the employee to the "Domain Admins" security group; log into DC-1 with that admin account.
</p>
<br />

3 - Join Client VM to the Active Directory Domain.
<p>
<img src="https://i.imgur.com/wGlVrmI.png" height="50%" width="50%"    alt="join client-1 to domain"/>
</p>
<p>
Next step is to join client-1 to the domain that was created. This will allow the created users or employees to log in, while using Client-1. Login to Client-1 in Remote Desktop and right-click the Windows icon (Start), and click System. Then click "Rename This PC (Advanced)".  Next click change, and enter the Domaine name. Enter the admin credential that was created on DC-1.  Once you click OK, an error message will appear.  The system reached out to the DNS Server to get the Domain Controler, but there is no Domain Control attached, so the DNS Servers needs to be changed to the Domain Controlers IP address.  
</p>
<br />

<p>
<img src="https://i.imgur.com/Ilp9boK.png" height="70%" width="70%" alt="client-1 creation"/>
</p>
<p>
To change the DNS Servers for Client-1 to the Domain Controls IP address, go to portal.azure.com.  Click on Virtual Machines, select DC-1, and click on Networking on the right. Copy the NIC Private IP address and go to Client-1.  Once in Client-1, click on Networking, click on Network Interface, click "DNS Severs", click custom, then enter DC-1's private IP.
</p>
<br />

<p>
<img src="https://i.imgur.com/28xtY50.png" height="50%" width="50%"    alt="join client-1 to domain"/>

Go back to Remote Desktop and login to Client-1.  Go to Search and open Command Prompt, type in "ipconfig /all" to check the DNS Servers to ensure the changes have been completed and the DC-1 IP address was updated.  Right-click Start, click System, click on Rename this PC (advanced), click on change, and enter the Domain name; for example: mydomain.com and click OK.

<img src="https://i.imgur.com/pCPbzzF.png" height="50%" width="50%" alt="allow domain users"/>
</p>
<p>
<img src="https://i.imgur.com/FumIpuQ.png" height="50%" width="50%" alt="non-Admin employee login"/>
</p>
<p>

4 - Setup remote desktop login for Admin and non-Admin users/ create user accounts.
<p>
<img src="https://i.imgur.com/zQthe6G.png" height="50%" width="50%"    alt="final login"/>
</p>
<p>
Lastly, we need to allow non-Admin users access to remote desktop. login to Client-1 using the admin account. Next, go to System, then click "remote desktop", then allow "domain users". The Active Directory is now completely set up and users can log in remotely using Client-1.
</p>
<br />
