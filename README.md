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

1) Creating Resource Group
2) Creating A Virtuall Network
3) Creating Your Domain Controller VM (Virtual Machine)
4) Creating A Client Virtual Machine
5) Making DC-1's IP Address (Static)
6) Firewall
7) Changing DNS Settings for "Client-1"
8) Pinging DC-1's Private IP Address
9) ipconfig /all


<h2>Deployment and Configuration Steps</h2>

<h3>1) Creating Resource Group</h3>

<p>Open up your Microsoft Azure and search for "Resource Group" in the search bar. Once you have selected "Resource Groups", click the top left button that says "Create". I will be naming my resource group "Active-Directory-Lab". Make sure to select your region to the corresponding region you are in (I'm in Canada, so my region would be Central Canada). Once you are done, you can simply review and create.</p>

<img src="https://i.imgur.com/Adulmc4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<h3>2) Creating A Virtuall Network</h3>

<p> For this step, you will go up to the search bar again and search up "Virtual Networks". After you have selected Virtual Networks you can begin the process by clicking the "Create" icon on the top left under "Virtual Networks". You will then be prompted to the "Create Virtual Network" screen where you will follow the same procedure as the one listed in step 2. Make sure you have selected the resource group you just created and make sure your region is the same as your resource group. I will be naming my virtual network "Active-Directory-Vnet". Once you have completed those steps, press "Review and Create" and "Create" on the bottom left of the screen. </p>

<img src="https://i.imgur.com/UMjl4mE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<h3>3) Creating Your Domain Controller VM (Virtual Machine) </h3>

<p>To start, search up "Virtual Machines" in your search bar and select. Select the "Create" button on the top left of the screen and click on "Azure virtual machine".  </p>

<img src="https://i.imgur.com/L0CveLE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<p> After you have done this, you will have to make sure you have these settings selected:</p> 
<ul>
  <li>Resource Group - Active-Director-Lab (Or the resource group you created in step 1)</li>
  <li>The same region you have selected for all your applications (For Example: mine is Central Canada)</li>
  <li>Image - (Windows Server 2022 Datacenter: Azure Edition - x64 Gen2)</li>
  <li>Size - At least 2 vcpus, 8 GiB memory</li>
  <li>Selected Inbound Ports - RDP (3389)</li>
</ul>

<img src="https://i.imgur.com/wRpF2OO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/GBW1G7T.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<p>Your username and password can be whatever you want as long as you write it down and remember it because we will be logging into this user later in the lab. Once you have finished these steps, if it is on the bottom of your starting page, check the "Licensing" box and then move on through the pages untill you get to the "Networking" page where you make sure that your virtual network that you just created is selected. You can then "Review and Create" and "Create". </p>

<h3>4) Creating A Client Virtual Machine</h3>

<p>This next step will be very similar to step 3 because we will be creating another "Virtual Machine". The only difference with this virtual machine is we will be naming it "client-1". This will essentially be a test virtual machine for us. Make sure these settings are selected:</p>

<ul>
  <li>Resource Group - Active-Director-Lab (Or the resource group you created in step 1)</li>
  <li>The same region you have selected for all your applications (For Example: mine is Central Canada)</li>
  <li>Image - (Windows 10 Pro, version 22H2 - x64 Gen2)</li>
  <li>Size - At least 2 vcpus, 8 GiB memory</li>
  <li>Selected Inbound Ports - RDP (3389)</li>
</ul>

<p> Make sure the license box is checked on the first page as well. </p>

<img src="https://i.imgur.com/srCWe7O.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/Susbj6e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<p>When you are done with those settings, go into the "Networking" tab and make sure:</p>
<ul>
  <li>Virtual Network: Active-Directory-Vnet</li>
  <li>Subnet: Default</li>
  <li>Public IP: Client-1 IP</li>
</ul>
<p>After you are done with these settings you can "Review and Create" -> "Create".</p>

<h3>5) Making DC-1's IP Address (Static)</h3>

<p>The reason we are making DC-1's IP "Static" is so that when we turn off our computer, our IP Address will stay the same. If we were to leave our IP Address for DC-1 on "Dynamic", their is a possibility that our IP address would change when we turn off our computer. To change our virtual machines ip address from "Static" to "Dynamic", first we would have to go into our DC-1 virtual machine by searching "Virtual Machine" and clicking on "DC-1" -> "Networking" -> "Network Settings" -> "Network Interface / ip configuration" (It'll have a green computer chip icon to the left of it) -> "ipconfig-1", make sure to select "Static" under "Private IP Adress Settings" then "Save".</p>

<img src="https://i.imgur.com/aQO5bjh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/mqji9V2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<h3>6) Firewall</h3>

<p>In most cases, it isn't safe to disable your firewall on your computer, but because this is simply a test "Virtual Machine" it is not going to create any harm. First we will have to log into our "DC-1" virtual machine. Open up your "Remote Destkop" software and make sure to copy the public ip address from this virtual machine. You can do this by searching up "Virtual Machine", clicking the "DC-1" VM you just created and looking for the public address displayed on the right side of the Virtual Machine tab.</p>

<img src="https://i.imgur.com/eSDoOof.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/4HcrXIA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<p>Once you are here, make sure to login using the username and password you created when we first made "DC-1". Once you have logged into your "DC-1" Virtual Desktop, right-click your windows icon on the bottom left, select "Run" and type in "wf.msc". You will see your windows firewall tab will open up once you have done this.</p>

<img src="https://i.imgur.com/Zj1Amaz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<p>In this window make sure to turn off your "Domain", "Private" and "Public" firewalls and then press "Apply" and "Ok".</p>

<img src="https://i.imgur.com/ZQ4ViI2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<h3>7) Changing DNS Settings for "Client-1"</h3>

<p>This next step will be copying "DC-1" private IP address and pasting it to "Client-1" in order for us to have these VM's properly connected. We will start by going back to your personal desktop and in the Azure browser, search for "Virtual Machine". Once you have searched this up, you can select "DC-1" and copy the "Private IP Address" on the bottom right side of the system settings.</p>

<img src="https://i.imgur.com/KZU2k7s.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<p>Once you have copied DC-s's Private IP Address, search for "Virtual Machines" -> Select "Client-1" -> go to "Network Settings" -> "Network Interface / IP configuration" -> "DNS Servers" -> "Custom" and Paste DC-1's private ip address. </p>

<img src="https://i.imgur.com/V5V5910.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<img src="https://i.imgur.com/Xywibzq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<h3>8) Pinging DC-1's Private IP Address</h3>

<p> In this next step we will be logging into our Client-1 virtual machine using our remote desktop connector to try and ping our dc-1 virtual machine. In order to do this, we will first need to be logged into "Client-1" on a remote desktop connector. Make sure to copy the "Public IP Address" for Client-1 (if you forget how to find your Public IP Address, you can go back to step 6 and re-read that section). Once you have copied your Public IP, you can then login using your username and password you created at the start of the lab for "Client-1". Once you have logged into "Client-1", on the windows search bar in the bottom left of the screen, type in "Power Shell" and click that. </p>

<img src="https://i.imgur.com/Ez5siOU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<p>Once your Power Shell is open, make sure to type in "ping" and whatever  DC-1's Private IP Address was (For Example: ping 10.0.0.4) into Power Shell. You will know you have done this right when you get four successful pings underneath your ping request. Your power shell should look like this if you have done everything right. </p>

<img src="https://i.imgur.com/Woc6zCX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<h3>9) ipconfig /all </h3>

<p>After you have successfully pinged, In the same powershell window type in "ipconfig /all". If your DNS server for "Client-1" is the same as "DC-1" (the private ip address you pasted earlier), then you have successfully applied Active Directory to your Virtual Machine! </p>

<h3>Congratulations!</h3>



