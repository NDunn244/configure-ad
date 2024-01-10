<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
In this lab, we will outline the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


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
You will first need to setup resources in Azure. Create a Domain Controller VM(Windows Server 2022) and name it "DC-1"
</p>
<p>
<img src="https://i.postimg.cc/28pPrmh5/Screen-Shot-2024-01-04-at-2-27-05-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Now, set the Domain Controller's NIC Private IP address to be static. This is to prevent the IP address from changing and it will remain the same.
</p>
<p>
<img src="https://i.postimg.cc/jSYcDSSD/Screen-Shot-2024-01-04-at-2-36-48-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Next, create a Client VM (Windows 10) named Client-1. You are going to use the same Resource Group and Vnet that was created for the Domain Controller VM.
</p>
<p>
<img src="https://i.postimg.cc/ZncfZk6f/Screen-Shot-2024-01-04-at-2-47-00-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Now, you will remote desktop into Client-1 and ping DC-1's private IP address with ping -t which is a perpetual ping.
</p>
<p>
<img src="https://i.postimg.cc/9fgJQ3g0/Screen-Shot-2024-01-04-at-3-07-01-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Next, Login to the Domain Controller and enable ICMPv4 on the local Windows firewall. There are two Core Networking Diagnosis you need to enable as shown below.
</p>
<p>
<img src="https://i.postimg.cc/c4WjjSpG/Screen-Shot-2024-01-04-at-3-16-34-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.postimg.cc/MpQQjCGX/Screen-Shot-2024-01-04-at-3-16-53-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Log back into Client-1 and you will now see the ping is successful.  
</p>
<p>
<img src="https://i.postimg.cc/6qx9gPXb/Screen-Shot-2024-01-04-at-3-21-12-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Next step is to install Active Directory Domain Services in DC-1. Click on Manage, then click on "Add Roles and Features", then select "Active Directory Domain Servies."
</p>
<p>
<img src="https://i.postimg.cc/rm160FBg/Screen-Shot-2024-01-04-at-3-31-41-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Then promote as a Domain Controller. 
</p>
<p>
<img src="https://i.postimg.cc/gJkyRg55/Screen-Shot-2024-01-04-at-3-35-33-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
You are going to set up a new forest as anything you want. Mine is named niadomain.com as shown below.
</p>
<p>
<img src="https://i.postimg.cc/G28Fq4LJ/Screen-Shot-2024-01-04-at-3-39-36-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
The DC-1 will restart automatically after the installation. Now you will log back into DC-1 as a user.  
</p>
<p>
<img src="https://i.postimg.cc/hPpXQSgS/Screen-Shot-2024-01-04-at-3-45-23-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Once you log back into DC-1, create an Admin and a Normal User Account in Active Directory. In tools, click on Users and Computers, then create an Organizational Unit called _EMPLOYEES and another called _ADMINS.  
</p>
<p>
<img src="https://i.postimg.cc/x8FSYFDS/Screen-Shot-2024-01-04-at-3-49-41-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.postimg.cc/WbQQnZsS/Screen-Shot-2024-01-04-at-3-51-17-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Create a new employee named "Jane Doe" with the username of "jane_admin". 
</p>
<p>
<img src="https://i.postimg.cc/Hk88qqDd/Screen-Shot-2024-01-04-at-3-55-05-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Add jane_admin to the Domain Admins Security Group. 
</p>
<p>
<img src="https://i.postimg.cc/YqGS0QXx/Screen-Shot-2024-01-04-at-3-59-11-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Now you are going to log out of DC-1 as "niadomain.com\labuser" and log back in as “niadomain.com\jane_admin”. You will use jane_admin as your admin account from now on. 
</p>
<p>
<img src="https://i.postimg.cc/g2pr7nDq/Screen-Shot-2024-01-04-at-4-02-50-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Head back to the Azure Portal and set Client-1's DNS settings to the DC's Private IP address.  
</p>
<p>
<img src="https://i.postimg.cc/wv8r5YHv/Screen-Shot-2024-01-04-at-4-07-40-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
In Azure Portal, restart Client-1. Log into Client-1 as the original local admin (labuser). Then go to settings, click on about, then click on "Rename this PC (advanced)" and join it to the domain. Once this is done, restart the computer.
</p>
<p>
<img src="https://i.postimg.cc/tJfWPpQv/Screen-Shot-2024-01-04-at-4-16-43-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Next, log into the Domain Controller and verify Client-1. Click on Active Directory Users and Computers, and inside the "Computers" container on the root of the domain you will find Client-1. Create a new Organizational Unit named _CLIENTS and drag Client-1 into there. 
</p>
<p>
<img src="https://i.postimg.cc/Hx8ygF7Q/Screen-Shot-2024-01-04-at-4-27-36-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Now, log into Client-1 as "niadomain.com\jane_admin" and open system properties. Click Remote Desktop and allow "domain users" access to remote desktop. You can now log into Client-1 as a normal, non-administrative user. Normally you'd want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab).  
</p>
<p>
<img src="https://i.postimg.cc/wvkd7jJH/Screen-Shot-2024-01-04-at-4-33-56-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Log into DC-1 as jane_admin. Open PowerShell_ise as an administrator. Create a new File and paste the contents of this script provided in the lab (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) into PowerShell. You can make some adjustments to the script. You can change how many users it would create from 10,000 to 100 as shown below. Then run the script to see all the accounts that are being created. 
</p>
<p>
<img src="https://i.postimg.cc/HnC4PTPJ/Screen-Shot-2024-01-04-at-4-45-04-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Once the script is done creating users, open Active Directory Users and Computers to observe the accounts in the appropriate OU and attempt to log into Client-1 with one of the accounts (take note of the password in the script). 
</p>
<p>
<img src="https://i.postimg.cc/pTBpRxL5/Screen-Shot-2024-01-04-at-4-47-50-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.postimg.cc/JhNh5mJY/Screen-Shot-2024-01-04-at-4-50-18-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
You are now logged into Client-1 as whatever account you choose that was created.
</p>
<br />
<p>
<img src="https://i.postimg.cc/rF7F6zNY/Screen-Shot-2024-01-04-at-4-50-53-PM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

