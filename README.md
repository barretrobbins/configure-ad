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

<h2>Operating Systems Used</h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1  - Setup Azure Domain Controller (windows server) and Azure Client (Windows 10) VMs.
- Step 2  - Install Active Directory in the Domain Controller.
- Step 3  - Join Client VM to the Active Directory Domain.
- Step 4  - Setup remote desktop login for Admin and non-Admin users/ create user accounts.

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/bdwisOl.png" height="80%" width="80%" alt="DC-1 setup"/>
</p>
<p>
First, create a Virtual Machine for the Domain Controler.  The region for the Domain Controler or DC-1 is based on selecting the region closest to your area.  Second, select Windows 22 Datacenter Server for the image.  Third, select Standard_E2s_v3-2vcpus, 16 Gib memory, and then scroll to the bottom, click on Disks->Networking-> Review + create-> and finally Create Virtual Machine DC-1.  

Next, create the second Virtual Machine for Client-1.  The region should be the same as the area selected for the Domain Controler (DC-1).  For this VM, the image is Windows 10 Pro, version 21H2-64 Gen2.  The size* will be the same as the first virtual machine (DC-1).  Scroll to the bottom, click on Disks
->Networking-> Review + create, and then Create Virtual Machine Client-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/2XVxIgi.png" height="80%" width="80%" alt="DC-1 with a static private IP address"/>
</p>
<p>
The following step is to change the NIC Private IP address for DC-1 to static instead of dynamic. Go to DC-1, click the Networking tab, click the Network Interface number, click the IP Configurations tab, click the Private IP address, and then change the assignment from dynamic to static.
</p>
<br />

<p>
<img src="https://i.imgur.com/cUAIEaz.png" height="80%" width="80%" alt="Active Directory"/>
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

<p>
<img src="https://i.imgur.com/Ilp9boK.png" height="70%" width="70%" alt="client-1 creation"/>
</p>
<p>
6. Next, create some non-Admin employees using the admin account. Once done, create a new VM in Azure with windows 10 as the operating system. Make sure the VM is in the same resource group and Vnet as DC-1. Name the VM client-1. Once the VM is created, in Azure, set Client-1's DNS settings to DC-1's Private IP address. Go to client-1's network interface settings, click "DNS Severs", click custom, then enter DC-1's private IP.
</p>
<br />

<p>
<img src="https://i.imgur.com/gVcLvBq.png" height="50%" width="50%"    alt="join client-1 to domain"/>
</p>
<p>
7. Now we need to join client-1 to the domain we created (campbell.com). This will allow our created users/employees to log in using client-1. Login to client-1, go to system, then click "Rename This PC(Advanced)", next click change, and enter the domain(campbell.com) you created in the domain field. You will need to enter the admin credentials we created on DC-1, for Example:(edward.admin@campbell.com). If done correctly, you should get a welcome to this domain pop-up message and a prompt to restart the VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/pCPbzzF.png" height="50%" width="50%"    alt="allow domain users"/>
</p>
<p>
<img src="https://i.imgur.com/FumIpuQ.png" height="50%" width="50%"    alt="non-Admin employee login"/>
</p>
<p>
  
<p>
<img src="https://i.imgur.com/zQthe6G.png" height="50%" width="50%"    alt="final login"/>
</p>
<p>
8. Lastly, we need to allow non-Admin users access to remote desktop. login to client-1 using the admin account. Next, go to System, then click "remote desktop", then allow "domain users". The Active Directory is now completely set up and users can log in remotely using client-1.
</p>
<br />
