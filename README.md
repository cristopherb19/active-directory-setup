<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Setup for Active Directory Infrastructure</h1>
Welcome!  This project serves as the first step to Active Directory implementation.  The objective of this project is to simulate an environment in Azure that allows the implementation of on-premises Active Directory.  We will be connecting two virtual machines that each serve their own purpose.  The first virtual machine will be the Domain Controller, and it will act as a server for the second virtual machine, the Client, to connect to.
<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Objectives</h2>

- Create and configure Domain Controller virtual machine
- Create Client virtual machine
- Establish a connection between the Domain Controller and Client virtual machines
- Ensure connectivity between the two virtual machines by inspecting network traffic

<h2>Configuration Steps</h2>

<h3>&#9312; Create Resource Group and Virtual Network</h3>
<p>

- In Azure, create a resource group
- Create a virtual network and select the previously created resource group 
<img src="https://imgur.com/WT0TuB7.png" height="80%" width="80%" alt="Virtual network creation"/>
<img src="https://imgur.com/bHSly6Q.png" height="80%" width="80%" alt="VM licensing"/>
  
</p>

<h3>&#9313; Create Domain Controller</h3>

<p>

- Create a virtual machine and name it "DC-1"
- Under "Image", select Windows Server 2022 Datacenter: Azure Edition


- Under "Size", select an option with at least 2 vcpus
- Choose and confirm "Username" and "Password" of your choice under "Administrator Account" section
<img src="https://imgur.com/XaJyHXQ.png" height="80%" width="80%" alt="VM licensing"/>

- Navigate to "Networking" tab
- Under "Virtual Network", select the virtual network created in the previous step
<img src="https://i.imgur.com/GmLc5OP.png" height="80%" width="80%" alt="Virtual network selection"/>

- Review + create your VM

</p>
<br />
<h3>&#9314; Create Client VM</h3>

<p>

- Create a virtual machine and name it "Client-1"
- Under "Image", select Windows 10 Pro
- Under "Size", select an option with at least 2 vcpus
- Choose and confirm "Username" and "Password" under "Administrator Account" section
- Confirm licensing
- Navigate to "Networking" tab
- Under "Virtual Network", select the same virtual network you did in the previous step
<img src="https://imgur.com/dngd9S1.png" height="80%" width="80%" alt="Client image"/>
<img src="https://i.imgur.com/GmLc5OP.png" height="80%" width="80%" alt="Virtual network selection"/>

- Review + create your VM
  
</p>
<br />

<h3>&#9315; Set Domain Controller's Private IP Address to "Static"</h3>

<p>

- Navigate to your DC-1 VM in Azure
- Navigate to "Network Settings"
- Click on "Network Interface / IP configuration" box
<img src="https://imgur.com/DncY5pb.png" height="80%" width="80%" alt="NIC"/>

- Click on "ipconfig1"
- Under "Allocation", select "Static"
<img src="https://imgur.com/WN6rdJa.png" height="80%" width="80%" alt="NIC"/>

- Click "Save" and now your Domain Controller's private IP address will not change
  
</p>
<br />

<h3>&#9316; Connect to the Domain Controller with Microsoft Remote Desktop App: If using Mac, install Windows App. </h3>

<p>

- Retrieve and copy public IP address of DC-1 VM
<img src="https://imgur.com/kYXljPN.png" height="80%" width="80%" alt="NIC"/>

- Paste public IP address into "PC Name" section of Remote Desktop and connect to the VM
<img src="https://imgur.com/mdyUYXJ.png" height="80%" width="80%" alt="NIC"/>
<img src="https://imgur.com/l2YZ3GM.png" height="80%" width="80%" alt="NIC"/>
- NOTE: If you don't see "Server Manager" application in your VM, it means you are either in the wrong VM or created the wrong type of VM
  
</p>
<br />

<h3>&#9317; Disable Firewalls in the Domain Controller</h3>

<p>

- Within the DC-1 VM, navigate to "Windows Defender FIrewall with Advanced Security"
- Click "Windows Defender Firewall Properties"
- Turn off "Firewall State" in "Domain Profile", "Private Profile", and "Public Profile" tabs
<img src="https://imgur.com/OfWat2p.png" height="80%" width="80%" alt="Firewall state"/>
<img src="https://imgur.com/0XpCgim.png" height="80%" width="80%" alt="Firewall state"/>
<img src="https://imgur.com/M97qS5Y.png" height="80%" width="80%" alt="Firewall state"/>
<img src="https://imgur.com/eFnQmQp.png" height="80%" width="80%" alt="Firewall state"/>

- Click "Apply" and "Ok"
- NOTE: Typically you probably wouldn't disable firewall settings, but for the sake of this project, we will disable them to prevent any complications. Another way to circumvent this is if you enable ICMP traffic in the firewall advanced settings.
  
</p>
<br />

<h3>&#9318; Connect Client VM to Domain Controller VM</h3>

<p>

- Retrieve DC-1 VM private IP address and copy it
<img src="https://imgur.com/OTUtsS0.png" height="80%" width="80%" alt="Firewall state"/>
- Navigate to Client-1 VM -> Network Settings -> click on "Network Interface / IP configuration" box
- On the left side of window, click on "DNS servers"
- Under "DNS servers", select the "Custom" option and paste the DC-1 private IP address
<img src="https://imgur.com/9EO5YBs.png" height="80%" width="80%" alt="DNS custom server"/>

- Click "Save"
- Navigate to "Virtual Machines" in Azure
- Select "Client-1" box
- Click "Restart"
<img src="https://imgur.com/boiVmFd.png" height="80%" width="80%" alt="Restart Client VM"/>
  
</p>
<br />

<h3>&#9319; Connect to Client VM with Remote Desktop</h3>

<p>

- Retrieve and copy public IP address of Client-1 VM
<img src="https://imgur.com/SaJIdH4.png" height="80%" width="80%" alt="Restart Client VM"/>
- Paste public IP address into "PC Name" section of Remote Desktop and connect to the VM
<img src="https://imgur.com/vvTEif0.png" height="80%" width="80%" alt="Restart Client VM"/>
<img src="https://imgur.com/Ef1PZMH.png" height="80%" width="80%" alt="Restart Client VM"/>
  
</p>
<br />

<h3>&#9320; Ensure Connectivity Between Domain Controller and Client</h3>

<p>

- Within the Client-1 VM, open PowerShell
- Ping the Domain Controller by typing "ping(private IP address)" (Example: ping 10.0.0.4)
- Observe and make sure the ping is successful
<img src="https://imgur.com/7kho6Rk.png" height="80%" width="80%" alt="Ping domain controller"/>
  
</p>

<h2>Conclusion</h2>

<p>
If you're reading this, then hopefully you have succesfully completed every step of this project.  This project has built the foundation for the following Active Directory projects. Now that our Domain Controller and Client are connected and configured, we can dig deeper into more advanced implementations and configurations of Active Directory.

- If you would like to continue to the next step in this series of Active Directory projects, please click <a href="https://github.com/cristopherb19/ad-deploy-and-config">here</a>

</p>
