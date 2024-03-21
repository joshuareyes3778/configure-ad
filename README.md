<p align="center">
<h1>Configuring Active Directory within Azure Virtual Machines</h1>
</p>

This tutorial outlines the setup of Active Directory within Azure virtual machines. <br />
<p> 
</p>

<h2> Environments and Technologies Used</h2>

- MIcrosoft Azure for Virtual Machines
- Remote Desktop Connection
- Active Directory Domain Services
- PowerShell

<h2> Operating Systems Used </h2>

- Windows Server 2022
- Windows 10

<h2>High-Level Deployment and Configuration Steps:</h2>

- Step 1: Creating the necessary resources in Azure (a Windows 10 VM and a Domain Controller with Windows 2022 Server)
- Step 2: Connecting the user VM to the Domain Controller
- Step 3: Installing Active Directory on the Windows Server
- Step 4: Creating Admin and user accounts in Active Directory
- Step 5: Joining the VM to a custom Domain
- Step 6: Creating several new Users that can log into the Windows 10 Client

<h2>Deployment and Configuration Steps</h2>

<p>
  To begin installing Active Directory, two virtual machines will be needed. One of them will be a Windows 10 machine with enough GPU's on it to let it operate smoothly. The other will be a Domain Controller.  Ensure that they are both created within the same resource group and network. During the setup of the DC, ensure that its NIC Private IP Address is set to Static. Once both of the machines are created and are in the same virtual network and resource group, they should be ready for connection.
</p>

<p></p>

<p></p>

<P></P>

<p></p>

<p></p>
