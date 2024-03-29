<p> align="center">
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

<h3>Creating Necessary Resources</h3>

<p>
  To begin installing Active Directory, two virtual machines will be needed. One of them will be a Windows 10 machine with enough GPU's on it to let it operate smoothly. The other will be a Domain Controller.  Ensure that they are both created within the same resource group and network. During the setup of the DC, ensure that its NIC Private IP Address is set to Static. Once both of the machines are created and are in the same virtual network and resource group, they should be ready for connection.
</p>
<br />

<h3>Connecting the User VM to the Domain Controller</h3>

<p>
  Once both machines are set up and active, the Windows 10 VM, from here onwards referred to as the Client vm, needs to be connected to the Domain Controller. Connect to the DC remotely and open Windows Firewall. There are two rules that need to be enabled, both of them being ICMP echo requests. The Client VM can be connected to at this point, and pings to the DC's private IP should be resolving. The DC is ready to have Active Directory installed on it.
</p>
<br />

<h3>Installing Active Directory onto the Domain Controller VM</h3>

<p>
  Now that the Client VM is able to communicate with the DC, the latter is ready to be promoted into a domain. To begin the process, click on the Add Roles and Features link in the Server Manager of the DC. Continue through the installation steps until you reach the Server Roles page. Active Directory Domain Services is the only extra role that must be selected, and then select Add Features on the resulting popup. From here, continue through the page and install Active Directory once the page comes up. Once this installs, a notification should appear on the server manager dashboard. This will allow the server to be promoted to a Domain Controller proper. Click on this, and select "Add a New Forest" on the page that comes up. The root domain is to also be named before the process can be continued. The next page requires that a password be created for Directory Services Restore Mode, but this will not be too important for this tutorial. Use whatever password desired and continue. Continue through the next pages and finish the promotion process, which will restart the DC. Since this will likely log you out, you will need to reconnect to the DC. This time however, the username needed to log into the DC will require the "Fully Qualified Domain Name." This will consist of [domainname].com\[username]. The password will remain the same. This username format will be used for all future login attempts into the DC. Now with the Domain Controller fully set up, users and admins can be created. 
</p>
<br />

<h3>Creating Admin and User Accounts</h3>

<P>
  With Active Directory properly installed on the DC, the admin account and some mock users can be created. To begin this process, Organizational Units must be created for the different users to be sorted into. The first one to be created will be "_EMPLOYEES." To find where to create this OU, first open Active Directory Users and Computers, and open the website domain resource group. It should have your domain's website listed as its name. Once inside this OU, the _EMPLOYEE OU can be created. A second OU named "_ADMINS" must also be created in the domain OU. With the OU's ready, users can be added into them as desired. Adding an admin user is as simple as going into the admins OU and creating a new user in the right click menu. Some credentials for the user will need to be created in the resulting menu. Make sure to uncheck the box that makes the user change their password at next login for the convenience of the lab. The user has been created, but lacks the necessary permissions to be an admin proper. Right click the user and select properties, and then Member Of. Then click on Add and select Domain Admins to add this user to. The Employees OU will be filled later on. With the OU's and the admin user created, the newly created credentials can be used to log into the DC. For the rest of the lab, log into the DC with these credentials.
</P>
<br />

<h3>Joining the User VM to the Custom Domain</h3>

<p>
   After logging back into the DC with the administrator credentials, the Client vm needs to be have its DNS settings set to the previously created domain. This process is done in Azure in the DNS settings for the client VM. This will be done in the Azure portal itself. First, the private IP of the DC will need to be copied. Then, in the client VM's DNS settings, change the DNS server to "Custom" and paste the DC's private IP into the bar. After this, the VM will need to be restarted and logged back into using the normal credentials. Go into the About in the system settings and click on "Rename This PC." Because the DNS of the client VM has been changed to use the DC's address, the domain name can now be put into the domain member text box. Then another popup will ask that the credentials of a user who can log into the DC. After completing this, the VM will restart. Currently, only domain admins are allowed to log onto the domain, but by going into the Remote Desktop system setting and adding "Domain Users," this will allow any domain user to log into the domain. Some mock users can be created to test this update out.
</p>
<br />

<h3>Creating Mock Users and Logging into the User VM</h3>

<p>
  The final task to observe how different users can log into the domain is to create some mock users. First, Powershell_ise must be run as an administrator on the DC to allow it to create users in the necessary OU. With Powershell open, insert this script (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) into PowerShell. Before running it, make sure that it matches the organizational units and other appropriate names to correctly create users in the designated employees OU. IF everything is set up correctly, randomly named users should be apearing as being created in PowerShell. These can also be seen in the employee OU as well. Using one of these random users, their username and password (should be password1 as created by the script) can be used to log into any computer connected to the DC. The DC can also be used to manage users in case they disable their account or can be used to disable accounts as well.
</p>
