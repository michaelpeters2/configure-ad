<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure
- Ensure Connectivity between the Client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in Active Directory
- Join Client-1 to Your Domain
- 

<h2>Deployment and Configuration Steps</h2>

  1) Create the Domain Controller Virtual Machine (Windows Server 2022) named "DC-1"
  - Take note of the Resource Group and Virtual Network (Vnet) that get created at this time. Also remember your username and password.
  2) Set Domain Controller’s NIC Private IP address to be static.
  
![image](https://github.com/michaelpeters2/configure-ad/assets/141062110/9b823be0-3d73-413f-aa59-8ece7d67d907)


  3) Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1a.
  4) Ensure that both VMs are in the same Vnet.
</p>

Ensure Connectivity between the Client and Domain Controller
-
  5) Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t (perpetual ping)
  6) Login to the Domain Controller with Remote Desktop and enable ICMPv4 in on the local windows Firewall.


![image](https://github.com/michaelpeters2/configure-ad/assets/141062110/3f37bb99-2851-4f1a-b562-c844ad8f8a0d)

  - On DC-1, search "firewall" and click on Windows Defender Firewall with Advanced Security.
  - Click Inbound Rules
  - Click Protocol and look for ICMPv4
  - Enable both Echo Requets
  7) Check back at Client-1 to see the ping succeed

Install Active Directory
-
  8) Login to DC-1 and install Active Directory Domain Services
    - On Server Manager, click Add Roles and Features
    - Click next three times
    - Click Active Directy Domain Services, add features
    - Click next until you see install.

![image](https://github.com/michaelpeters2/configure-ad/assets/141062110/49a535e6-53e3-4d42-b925-bbe703704e2a)
</p>
  9) Promote this server to a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
    - Password: Password1
    - Click next all the way to install
  10) Restart and then log back into DC-1 as user: mydomain.com\labuser (May have to click "more options")

![image](https://github.com/michaelpeters2/configure-ad/assets/141062110/a15cbad4-693b-4702-aeba-9204a656243b)


  Create an Admin and Normal User Account in AD
  -
  11) Click Tools, then in Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”

![image](https://github.com/michaelpeters2/configure-ad/assets/141062110/726e6dc0-c95d-4828-a327-bf5c30097b3d)

  12) Create a new OU named “_ADMINS”
  13) Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
  - Go to _ADMINS folder -> New -> User

![image](https://github.com/michaelpeters2/configure-ad/assets/141062110/77e99e69-7cbd-41f0-85f4-985124b09d86)

  14) Add jane_admin to the “Domain Admins” Security Group
  - Right click Jane Doe user and go to properties -> member of -> add domain admins
  15) Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
  16) User jane_admin as your admin account from now on
![image](https://github.com/michaelpeters2/configure-ad/assets/141062110/97f79fdd-908c-4c6d-806d-f5bf74a2a766)

Join Client-1 to Your Domain (mydomain.com)
-
  17) From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
  18) 
