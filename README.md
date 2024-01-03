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

<h2>Deployment and Configuration Steps</h2>

Step 1:

![image]()

Creating the domain controller virtual machine
- Navigate to https://portal.azure.com/ and create a VM and label it as DC-1 as the domain controller
- When creating the VM, make sure its the image of a Windows Server 2022
- Configure it to 2 vcpus
- Make note of the username and password created
- Click on review and create
- Once creation is done, go to networking tab then click on the "network interface" then IP configurations then select ipconfig1 and change to static and save

Step 2:

![image]()

Creating the client VM
- Create a new VM and label it as Client-1
- Use Windows 10 pro with 2 vcpus
- Make sure to use the same resource group and vnet that the DC-1 VM is using
- For the DC-1 vnet to show up in the creation of Client-1's VM, you might have to wait 5-10 minutes for it to populate
- TO ensure both VMs are in the same vnet, you can click on the overview of the VMs in azure

Step 3:

![image]()

Testing Connections
- Login into client-1 and ping DC-1 IP address with "ping -t" means infinite connection testing
- To ping, copy DC-1's private IP address from the azure portal
- Navigate to the command prompt by looking up "CMD" in the search bar
- Type ping -t "DC1 private IP address" you will then see request timed out as there isn't an open connection to each other

Step 4:

![image]()

Opening the firewall

