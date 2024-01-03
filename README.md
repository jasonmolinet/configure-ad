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
- Windows 10 Pro

<h2>Deployment and Configuration Steps</h2>

Step 1:

![image](https://i.imgur.com/zmUeU34.jpg)

Creating the domain controller virtual machine
- Navigate to https://portal.azure.com/ and create a VM and label it as DC-1 as the domain controller
- When creating the VM, make sure its the image of a Windows Server 2022
- Configure it to 2 vcpus
- Make note of the username and password created
- Click on review and create
- Once creation is done, go to networking tab then click on the "network interface" then IP configurations then select ipconfig1 and change to static and save

Step 2:

![image](https://i.imgur.com/EmWrF9P.jpg)

Creating the client VM
- Create a new VM and label it as Client-1
- Use Windows 10 pro with 2 vcpus
- Make sure to use the same resource group and vnet that the DC-1 VM is using
- For the DC-1 vnet to show up in the creation of Client-1's VM, you might have to wait 5-10 minutes for it to populate
- TO ensure both VMs are in the same vnet, you can click on the overview of the VMs in azure

Step 3:

![image](https://i.imgur.com/SFzICM7.jpg)

Testing Connections
- Login into client-1 and ping DC-1 IP address with "ping -t" means infinite connection testing
- To ping, copy DC-1's private IP address from the azure portal
- Navigate to the command prompt by looking up "CMD" in the search bar
- Type ping -t "DC1 private IP address" you will then see request timed out as there isn't an open connection to each other

Step 4:

![image](https://i.imgur.com/bJOj3i5.jpg)

Opening the firewall
- To open that firewall connection, lets go to DC-1 VM via remote desktop
- Once the DC-1 VM is open, type wf.msc to the search bar
- Sort by protocol and find ICMPv4
- Enable the rule for the two rules that state "Core Networking Diagnostics ICMP"
- Go back to client-1 VM and see it ping and display a connection between client-1 and DC-1
- Hitting control and C in the CMD it will stop the constant pinging

Step 5:

![image](https://i.imgur.com/T18uPM9.jpg)

Installing Active Directory
- Now we got to install active directory by going to DC-1 VM and under server manger go to add roles and features
- Make sure under server roles you select "Active Directory Domain Services"
- After installation is complete, you got to the flag in the top right and hit promote to finalize the process
- Hit add a new forest and label it as mydomain.com
- Password can be Password1
- After going through the installation, its going to restart and you have to reconnect to DC-1 VM again via remote desktop

Step 6:

![image](https://i.imgur.com/rT8VXam.jpg)

Admin account and folders
- Since we turn the DC-1 VM as a domain controller when logging in we have to use username as "mydomain.com\labuser" or the \username created and the password is the same as it was created in the VM setup
- Login and hit the search for active directory users and computers
- Create two folders by right clicking mydomain.com folder and select new then organization unit.
- Label them _ADMINS and _EMPLOYEES
- Create a user called jane doe and make the password to never expire then right click on the user and hit properties and under member of tab then hit add and type domain and check names from there select domain admins and apply
- Log out of DC-1 and log back in as mydomain.com\jane_admin
- Once logged as Jane, you can look at the CMD and type "whoami" to show that you are logged as jane_admin

Step 7:

![image](https://i.imgur.com/AX8AtuP.jpg)

Client-1 connection to the domain
- Go to azure and copy DC-1 private IP address then go to client-1 and click network interface under networking then DNS server tab and hit custom and paste DC-1 private IP address there
- Once clicking save is done, we can restart client-1 in azure
- Relog into client-1 go to CMD and ipconfig/all you can then see that the DNS server IP address is the same as DC-1 private IP
- Now we can right click on start and hit system from there hit rename this PC(advanced) then change and check domain and type out the domain as mydomain.com then type the username and password as mydomain.com\jane_admin and password as the one created in DC-1
- After a restart of client-1 VM, as client 1 is a member of the domain we can login as mydomain.com\jane_admin
- Once login go to system then remote desktop and select users at the bottom 
- Hit add then type "domain users" and hit check names then ok
- Now go back to DC-1 VM and open active directory users and computers
- Click on the "computers" folder
- Verify Client-1 shows up in there

Step 8:

![image](https://i.imgur.com/C55Oiwa.jpg)

Adding new users
- Now in DC-1 VM open Powershell ISE as administrator 
- Create a new file and paste a script provided here: https://github.com/jasonmolinet/configure-ad/blob/main/generate-names
- Then run the script with the play button
- While the accounts are creating go to the _EMPLOYEES folder and copy a random one and go to client 1 VM and attempt to log in as that random user
- If you go to the C: then users you can see the users log into that computer for example client 1
- Now log out and find a new user from DC-1 and copy their display name as we will attempt to show what happens when someone logs in and fails too many times
- Then go into DC-1 and right click on the user and hit reset password and make a new password but also check unlock account, you can also go to properties by right clicking and then account and hit unlock
- You can also right click and disable account and then try to log in and you'll get an error 
- Then right click and re-enable the account
