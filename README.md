<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>

<p>
</p>
<p>
Firstly, creat 2 VMs (Virtual Machines) within the same VNET (Virtual Networ). One will be a "Domain Controller" and the other will be a Client machine. Change the DC to a static IP because it'll be offering Active Directory services to the Client machine. The Client machine will be joined to the Domain Controller. You'll control the DNS settings on the client machine and the client machine will use the DC as its DNS server. 
</p>
<br />

<p>
<img src="https://i.imgur.com/d22FHIm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
As a reminder, DC-1 must have a static Private IP Address. Client one will connect to DC-1 . To ensure connectivity, ping DC-1 from Client-1. Upon first attempt, the ping will fail. This means you have to enable ICMPv4 on the firewall on DC-1 and you'll be able to successfully ping DC-1 from Client-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/HvZBWzc.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/1lrrGPw.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
Log back into DC-1 to install Acive Directory Users & Computers. Promote the VM to DC, setup a new forest as "mydomain.com". Then, restart and log back into DC-1 as user: "mydomain.com\labuser". If you performed the steps correctly you should be able to run Active Directory Users & Computers as shown below.
</p>
<img src="https://i.imgur.com/cGjvRke.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
Now you can begin creating Organizational Units (OU). Start by creating an OU named _EMPLOYEES. Then create another OU named _ADMINS. In order to do this, right click on the domain area and select New > Organizational Unit and fill out the field. Then click inside of your OU and right click and select "New" and select "User" and fill out the information for your new user. Name the example user Jane Doe, she is going to be an Admin so her username will be Jane_admin. Lastly add Jane to the domain admins security group. 
</p>
<img src="https://i.imgur.com/hL7g5Y5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kcgvzdE.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
Jane_admin will be used as the the administrator account. Next, join Client-1 to the domain (mydomain.com) from the azure portal and change client-1's DNS settings to the DC's Private IP address. Restart Client-1 from within the Azure portal. (Photo below shows verification that client-1 is on the DC-1 DNS) 
</p>
<img src="https://i.imgur.com/jbrGTXW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kvcm2cY.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<p>
Join Client-1 to the domain. To do this, navigate to your system settings and go to "About". Select "Rename this PC (advanced)". From there select to change the domain. Enter "mydomain.com" then enter your credentials from mydomain.com\labuser. Your computer will restart and client-1 will be a part of mydomain.com
</p>
<br />
<p>
  <p>
<img src="https://i.imgur.com/Ze0Em5e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Client-1 is now a part of the domain. Next, set up remote desktop for non-administrative users on Client-1. Do this by logging into Client-1 as an admin and opening system properties. Click on "Remote Desktop", allow "domain users" access to remote desktop. After completing those steps log into Client-1 as a normal user.
</p>
<br />

<p>
  <p>
<img src="https://i.imgur.com/SApOKiE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly, to verify that noraml users can RDP into Client-1, use a script to generate thousands of users into the domain. Input the script using powershell and after the users are created we will select one from the bunch and RDP into Client-1.
</p>
<br />
<img src="https://i.imgur.com/EzWG8ug.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<p>
  <p>
<img src="https://i.imgur.com/Gkpe68K.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/n3gMwQV.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
The Powershell script created a user with the username "bab.hubo" and access to Client-1 has been gained using it's credentials. 
</p>
