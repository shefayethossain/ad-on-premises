<p align="center">
<img src="https://i.imgur.com/pmz4rhy.png" alt="img"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This lab provides a step-by-step guide to implementing Active Directory on Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro (22H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure (DC-1 VM and Client-1 VM)
- Set DC-1 NIC private IP from dynamic to static
- Log into Client-1 VM via Remote Desktop and ping DC-1's IP address
- Log into DC-1 VM via Remote Desktop and allow ICMP traffic
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory on DC-1 (promote to Domain Controller)
- Reconnect to DC-1 as Domain Controller (mydomain.com\labuser)
- Create OUs named _EMPLOYEES and _ADMINS
- Create employee named jane doe (jane_admin)
- Add jane_admin to the Domain Admins security group
- Log out of DC-1 and log back in as "mydomain.com\jane_admin
- Set Client-1's DNS settings to the Domain Controllers (DC-1) private IP address
- Join Client-1 to your domain (mydomain.com)
- Log into Client-1's VM as jane doe (mydomain.com\jane_admin)
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<p align="center">
<img src="https://i.imgur.com/lNFfgp4.png" height="80%" width="80%" alt="img"/>
</p>

Go to https://portal.azure.com/

Search for "virtual machine in the search bar and click "Virtual machines".

<p align="center">
<img src="https://i.imgur.com/vir8b1g.png" height="80%" width="80%" alt="img"/>
</p>

Click "Create", and click "Azure virtual machine".

<p align="center">
<img src="https://i.imgur.com/CIbAwEw.png" height="80%" width="80%" alt="img"/>
</p>

Select your Azure subscription, click "create new, and name your resource group "AD-Lab". Name your virtual machine "DC-1" and select (US) West 3 US for the region. For availability options, select "No infrastructure redundancy required". Select "Standard" for security type, and select "Windows Server 2022 dtatcenter: Azure Edition" for image. For size, select "Standard_E2s_v3 - 2 vcpus, 16 GiB memory". Use "labuser" as your username, and input a unique password. Click "Review + creat".

<p align="center">
<img src="https://i.imgur.com/Wl2u6Ll.png" height="80%" width="80%" alt="img"/>
</p>

We got a "Valiadation passed" message, click the "Create" button in the bottom left.

<p align="center">
<img src="https://i.imgur.com/zYlGl4p.png" height="80%" width="80%" alt="img"/>
</p>

The "Your deployment is complete" message indicates that our DC-1 VM has been created.

Let's go ahead and create "Client-1" VM.

<p align="center">
<img src="https://i.imgur.com/vir8b1g.png" height="80%" width="80%" alt="img"/>
</p>

Go back virtual machine, click "Create", and click "Azure virtual machine".

<p align="center">
<img src="https://i.imgur.com/Z3MrTxB.png" height="80%" width="80%" alt="img"/>
</p>

Select your Azure subsription, select the same resource group as DC-1, name your virtual machine "Client-1", select the same region, availability options, and security type as DC-1. Select "Windows 10 Pro, version 22H2" for image. Select the same size and use the same username and password you used for DC-1. Check the licensing box and click the networking tab at the top.

<p align="center">
<img src="https://i.imgur.com/ZCNkJ5o.png" height="80%" width="80%" alt="img"/>
</p>

Make sure you select the same virtual network as DC-1. A subnet and IP address will automatically be created for you. Click the "Review + create" button in the bottom left.

<p align="center">
<img src="https://i.imgur.com/qZHlkOS.png" height="80%" width="80%" alt="img"/>
</p>

We got a "Valiadation passed" message. Go ahead and click the "Create" button in the bottom left.

<p align="center">
<img src="https://i.imgur.com/pLTETK3.png" height="80%" width="80%" alt="img"/>
</p>

The "Your deployment is complete" message indicates that our Client-1 VM has been created.

<p align="center">
<img src="https://i.imgur.com/9kpDOrS.png" height="80%" width="80%" alt="img"/>
</p>

We will now set DC-1 NIC private IP from dynamic to static. Go back to virtual machine and click "DC-1".

<p align="center">
<img src="https://i.imgur.com/Ko8EfOS.png" height="80%" width="80%" alt="img"/>
</p>

Click "Networking", then click DC-1 Network Interface.

<p align="center">
<img src="https://i.imgur.com/6m9XTCx.png" height="80%" width="80%" alt="img"/>
</p>

Click "Ip Configurations".

<p align="center">
<img src="https://i.imgur.com/nGTecei.png" height="80%" width="80%" alt="img"/>
</p>

Click "ipconfig1".

<p align="center">
<img src="https://i.imgur.com/wo6rDCo.png" height="80%" width="80%" alt="img"/>
</p>

Select "Static", and click the "Save" button. This means that the IP address of DC-1 will not change.

<p align="center">
<img src="https://i.imgur.com/FWvuMXc.png" height="80%" width="80%" alt="img"/>
</p>

Log into Client-1 VM via Remote Desktop and ping DC-1's IP address (perpetual ping).

Go to virtual machine and click "Client-1".

<p align="center">
<img src="https://i.imgur.com/J73jcNu.png" height="80%" width="80%" alt="img"/>
</p>

Copy Client-1's public IP address.

<p align="center">
<img src="https://i.imgur.com/nVjRq04.png" height="80%" width="80%" alt="img"/>
</p>

Open Remote Desktop, paste Client-1's IP address, and click "Connect".

<p align="center">
<img src="https://i.imgur.com/RHIfVY8.png" height="80%" width="80%" alt="img"/>
</p>

Click "More choice" > "Use a different account", type in the username and password we used when we were creating Client-1's VM, and click the "Ok" button.

Minimize Client-1's VM.

<p align="center">
<img src="https://i.imgur.com/EggybQN.png" height="80%" width="80%" alt="img"/>
</p>

In your Azure portal, click DC-1.

<p align="center">
<img src="https://i.imgur.com/tjATsm7.png" height="80%" width="80%" alt="img"/>
</p>

Note down DC-1's Private IP Address.

<p align="center">
<img src="https://i.imgur.com/ypTy31O.png" height="80%" width="80%" alt="img"/>
</p>

Go back to Client-1's VM, select "No" for all the options, and click the "Accept" button.

<p align="center">
<img src="https://i.imgur.com/lc45ZoW.png" height="80%" width="80%" alt="img"/>
</p>

On the search box, type "cmd", and click "Open".

<p align="center">
<img src="https://i.imgur.com/BMwpabu.png" height="80%" width="80%" alt="img"/>
</p>

Type "ping -t 10.0.0.4" to ping DC-1's private IP address.

As shown in the image above, the ping got timed out. This is because DC-1's Windows firewall is blocking ICMP traffic.

Go ahead and minimize Client-1's VM.

<p align="center">
<img src="https://i.imgur.com/Fz35nGv.png" height="80%" width="80%" alt="img"/>
</p>

Let's log into DC-1 via Remote Desktop.

Go back to your Azure portal and copy DC-1's Public IP address.

<p align="center">
<img src="https://i.imgur.com/dz8DfzZ.png" height="80%" width="80%" alt="img"/>
</p>

Open Remote Desktop and log into DC-1's VM by pasting the public Ip address and clicking "Connect". 

<p align="center">
<img src="https://i.imgur.com/1rDG2c0.png" height="80%" width="80%" alt="img"/>
</p>

Log in just like we did for Client-1.

<p align="center">
<img src="https://i.imgur.com/vXbs4sG.png" height="80%" width="80%" alt="img"/>
</p>

Click the "Yes" button.

<p align="center">
<img src="https://i.imgur.com/EEtpuNt.png" height="80%" width="80%" alt="img"/>
</p>

Let's allow ICMP traffic on DC-1.

In the search box, type "wf.msc", and click on it.

<p align="center">
<img src="https://i.imgur.com/WdzS9zC.png" height="80%" width="80%" alt="img"/>
</p>

Click "Inbound Rule" in the left pane, then click "Protocol" to sort by protocol. Right-click on both "ICMPv4-in" echo requests and click "Enable rule".

<p align="center">
<img src="https://i.imgur.com/DKsGL44.png" height="80%" width="80%" alt="img"/>
</p>

Go back to Client-1's VM and observe that the pings are now working after we enabled the ICMP echo request on DC-1. Press "CTRL + C" on your keyboard to stop the perpetual ping, close the cmd application by clicking "X", and minimize Client-1's VM.

NOTE: We did this to ensure that Client-1 and DC-1 could communicate with each other.

Next, let's install Active Directory on DC-1.

<p align="center">
<img src="https://i.imgur.com/OmwrUEy.png" height="80%" width="80%" alt="img"/>
</p>

NOTE: Just so you don't get confused between DC-1 VM and Client-1 VM, click on any VM, open cmd, and type "hostname", Click Enter.

As shown in the image above, we are on DC-1's VM.

<p align="center">
<img src="https://i.imgur.com/Nvlpnnf.png" height="80%" width="80%" alt="img"/>
</p>

On DC-1 Vm, click the Start menu and click "Server Manager".

<p align="center">
<img src="https://i.imgur.com/h7rSD6a.png" height="80%" width="80%" alt="img"/>
</p>

Click "Add roles and features".

<p align="center">
<img src="https://i.imgur.com/tTpSsl5.png" height="80%" width="80%" alt="img"/>
</p>

Click "Next" > "Next" > "Next".

<p align="center">
<img src="https://i.imgur.com/EahtxUJ.png" height="80%" width="80%" alt="img"/>
</p>

Click "Active Directory Domain Services". 

<p align="center">
<img src="https://i.imgur.com/KwJPlmY.png" height="80%" width="80%" alt="img"/>
</p>

A new window will pop up. Click "Add Features".

<p align="center">
<img src="https://i.imgur.com/tapsNSk.png" height="80%" width="80%" alt="img"/>
</p>

In the next few windows, click "Next" > "Next" > "Next" > "Install".

<p align="center">
<img src="https://i.imgur.com/KEEsm8x.png" height="80%" width="80%" alt="img"/>
</p>

When Active Directory is done installing, click the "close" button.

<p align="center">
<img src="https://i.imgur.com/zBgtUvK.png" height="80%" width="80%" alt="img"/>
</p>

On Server Manager, click the flag icon and click "Promote this server to a domain controller".

<p align="center">
<img src="https://i.imgur.com/WfwcBlA.png" height="80%" width="80%" alt="img"/>
</p>

A new window will pop up. Select "Add a new forcast" and name your root domain "mydomain.com" (you can change this to your name if you want). Click "Next".

<p align="center">
<img src="https://i.imgur.com/m83D8tC.png" height="80%" width="80%" alt="img"/>
</p>

Type "Password1" in the Password and Confirm Password box, and click "Next" > "Next" > "Next" > "Next" > "Next" > "Install". 

NOTE: After Active Directory is installed, you will be disconnected from DC-1's VM. If this happens, just go back your Azure portal, grab your DC-1's public IP address.

<p align="center">
<img src="https://i.imgur.com/9scInGF.png" height="80%" width="80%" alt="img"/>
</p>

Open Remote Deskstop, paste DC-1's public IP address, and click "Connect".

<p align="center">
<img src="https://i.imgur.com/Rl9yLBg.png" height="80%" width="80%" alt="img"/>
</p>

Click "More choices" > "Use a different account". 

Since DC-1 is now a Domain Controller, we will log in using FQDN (Fully Qualified Domain Name). Type in "mydomain.com\labuser" as username and the password we used when we were creating DC-1's VM in Azure and click "Ok".

<p align="center">
<img src="https://i.imgur.com/NA9QeOz.png" height="80%" width="80%" alt="img"/>
</p>

Click "Yes".

<p align="center">
<img src="https://i.imgur.com/TinijCf.png" height="80%" width="80%" alt="img"/>
</p>

Type "active directory" in the serach box, and click "Active Directory Users nad Computers".

<p align="center">
<img src="https://i.imgur.com/YBlzBaD.png" height="80%" width="80%" alt="img"/>
</p>

We will go ahead and create our Organizational Units (OU).

As shown in the image above, right-click "mydomain.com", click "New", and click "Organizational Unit".

<p align="center">
<img src="https://i.imgur.com/eshE7Kr.png" height="80%" width="80%" alt="img"/>
</p>

On the new window, type "_EMPLOYEES", and click "Ok".

<p align="center">
<img src="https://i.imgur.com/YBlzBaD.png" height="80%" width="80%" alt="img"/>
</p>

Let's create another one.

Right-click "mydomain.com", click "New", and click "Organizational Unit".

<p align="center">
<img src="https://i.imgur.com/tlwKpYF.png" height="80%" width="80%" alt="img"/>
</p>

On the new window, type "_ADMINS", and click "Ok".

<p align="center">
<img src="https://i.imgur.com/tJl1qZv.png" height="80%" width="80%" alt="img"/>
</p>

Right-click "mydomain.com, and click "Refresh". As shown in the image above, you can see the two OUs we created are now at the top.

<p align="center">
<img src="https://i.imgur.com/VKKkYiO.png" height="80%" width="80%" alt="img"/>
</p>

Right-click "Users", You will notice that we are currently signed into DC-1 as "labuser".

<p align="center">
<img src="https://i.imgur.com/8IQmy0o.png" height="80%" width="80%" alt="img"/>
</p>

We will create another administrative account that's tied to us as individuals, and then we will log out and log back in using the new administrative account (jane_admin).

Click "_ADMINS", right-click on the empty space, and click "New" > "User".

<p align="center">
<img src="https://i.imgur.com/9F9piEj.png" height="80%" width="80%" alt="img"/>
</p>

Use "jane" as the first name, "doe" as the last name, and type in the full name. Use "jane_admin" as the login name and click Next.

<p align="center">
<img src="https://i.imgur.com/pQ62Vxu.png" height="80%" width="80%" alt="img"/>
</p>

We will use "Password1" as the password, only check "Password never expires" box, and click "Next" > "Finish".

<p align="center">
<img src="https://i.imgur.com/aidlPL1.png" height="80%" width="80%" alt="img"/>
</p>

Let's make "jane doe" a domain admin by assigning it to the domain admins group.

Right-click "jane doe" and click "Properties".

<p align="center">
<img src="https://i.imgur.com/E1AypOm.png" height="80%" width="80%" alt="img"/>
</p>

Click "Member Of" > "Add", type "Domain Admins" in the box, and click "Check name" > "Ok" > "Apply" > "Ok".

<p align="center">
<img src="https://i.imgur.com/PulUoYG.png" height="80%" width="80%" alt="img"/>
</p>

Before we log out, opem command prompt and type "whoami" and press Enter, as show in the image above.

We are signed in as "mydomain\labuser".

<p align="center">
<img src="https://i.imgur.com/guyoQEC.png" height="80%" width="80%" alt="img"/>
</p>

Type "logoff" and press Enter to sign out from "mydomain\labuser".

<p align="center">
<img src="https://i.imgur.com/yve1bwx.png" height="80%" width="80%" alt="img"/>
</p>

Let's log back in using the new administrative account (jane_admin). 

Go to your Azure portal and grab DC-1's public IP Address

<p align="center">
<img src="https://i.imgur.com/dz8DfzZ.png" height="80%" width="80%" alt="img"/>
</p>

Paste the public IP address and click "Connect".

<p align="center">
<img src="https://i.imgur.com/0c272w2.png" height="80%" width="80%" alt="img"/>
</p>

Click "More choices" > "Use a different account", use "mydomain.com\jane_admin" as the username, type the password we created for jane_admin (Password1), and click "Ok".

<p align="center">
<img src="https://i.imgur.com/NA9QeOz.png" height="80%" width="80%" alt="img"/>
</p>

Click "Yes".

<p align="center">
<img src="https://i.imgur.com/OSKWWYG.png" height="80%" width="80%" alt="img"/>
</p>

We are now signed as jane doe. To confirm, open the command prompt, type "whoami", and click Enter.

As shown in the image above, we are signed in as "jane_admin (jane doe) who is a member of "mydomain".

Type "hostname" and click Enter. You can see we are in DC-1 VM. Exit out of the command prompt and minimize DC-1's VM.

<p align="center">
<img src="https://i.imgur.com/7XcnGk6.png" height="80%" width="80%" alt="img"/>
</p>

Next, let's set Client-1's DNS settings to point to the Domain Controllers (DC-1) private IP address.

This will let Client-1 join DC-1's domain (mydomain.com). Thereby letting us log into Client-1's VM as "jane doe" (jane_admin)

NOTE: Currently, Client-1's DNS is pointing to the Azure-assigned DNS server. To join DC-1's domain (mydomain.com), we need to configure Client-1 to use DC-1's private IP address as its DNS server instead. This is because the domain controller (DC-1) knows what "mydomain.com is.

Before we configure Client-1's DNS, let's attempt to join it to the domain. Let's log into Client-1 as the original admin account (labuser).

As shown in the image above, go to Azure portal and copy Client-1's public IP address.

<p align="center">
<img src="https://i.imgur.com/ozCVl6k.png" height="80%" width="80%" alt="img"/>
</p>

Open Remote Deskstop, paste Client-1's public IP address, and click "Connect".

<p align="center">
<img src="https://i.imgur.com/pXeOCVf.png" height="80%" width="80%" alt="img"/>
</p>

Type in the password and click "Ok".

<p align="center">
<img src="https://i.imgur.com/p6Qb1ny.png" height="80%" width="80%" alt="img"/>
</p>

Click "Yes".

<p align="center">
<img src="https://i.imgur.com/hqBvyEB.png" height="80%" width="80%" alt="img"/>
</p>

To join the domain, right-click the Start Menu and click "System".

<p align="center">
<img src="https://i.imgur.com/NlvckON.png" height="80%" width="80%" alt="img"/>
</p>

Click "Rename this PC (Advanced)" > "Change". Select "Domain", type in the box "mydomain.com", and click "Ok".

NOTE: I mistakenly typed "domain.com" instead of "mydomain.com". But we still get the message below regardless.

We got a message saying "mydomain.com could not be contacted". Click "Ok" > "Cancel" > "Cancel".

<p align="center">
<img src="https://i.imgur.com/EggybQN.png" height="80%" width="80%" alt="img"/>
</p>

Let's now configure Client-1 to use DC-1's private IP address as its DNS server.

Go to your Azure portal. In virtual machines, click "DC-1".

<p align="center">
<img src="https://i.imgur.com/gViKx2h.png" height="80%" width="80%" alt="img"/>
</p>

Copy DC-1's private IP address.

<p align="center">
<img src="https://i.imgur.com/FWvuMXc.png" height="80%" width="80%" alt="img"/>
</p>

Go back virtual machine and click "Client-1".

<p align="center">
<img src="https://i.imgur.com/DoBQ3O6.png" height="80%" width="80%" alt="img"/>
</p>

Click "Networking".

<p align="center">
<img src="https://i.imgur.com/hKcQAmB.png" height="80%" width="80%" alt="img"/>
</p>

In Networking, click Client-1's Network Interface.

<p align="center">
<img src="https://i.imgur.com/ntpngRl.png" height="80%" width="80%" alt="img"/>
</p>

Click "DNS Servers".

<p align="center">
<img src="https://i.imgur.com/tYBma4J.png" height="80%" width="80%" alt="img"/>
</p>

Select "Custom", paste DC-1's private IP address, and click "Save".

We've now configured Client-1's DNS to DC-1's private IP.

<p align="center">
<img src="https://i.imgur.com/FWvuMXc.png" height="80%" width="80%" alt="img"/>
</p>

Go back to virtual machines and click "Client-1".

<p align="center">
<img src="https://i.imgur.com/N0sH6bW.png" height="80%" width="80%" alt="img"/>
</p>

Click "Restart" and click "Yes" at the prompt (this will flush Client-1's DNS cache).

<p align="center">
<img src="https://i.imgur.com/7XcnGk6.png" height="80%" width="80%" alt="img"/>
</p>

Let's re-attempt to join Client-1 to the DC-1 domain. 

As shown in the image above, go to Azure portal and copy Client-1's public IP address.

<p align="center">
<img src="https://i.imgur.com/ozCVl6k.png" height="80%" width="80%" alt="img"/>
</p>

Open Remote Deskstop, paste Client-1's public IP address, and click "Connect".

<p align="center">
<img src="https://i.imgur.com/pXeOCVf.png" height="80%" width="80%" alt="img"/>
</p>

Type in the password and click "Ok".

<p align="center">
<img src="https://i.imgur.com/p6Qb1ny.png" height="80%" width="80%" alt="img"/>
</p>

Click "Yes".

<p align="center">
<img src="https://i.imgur.com/qIVbBRv.png" height="80%" width="80%" alt="img"/>
</p>

Open command prompt and type "ipconfig /all". 

As shown in the image above, you can see that Client-1's DNS server has now been configured to DC-1's private IP address.

Ping the IP address "ping 10.0.0.4". We got replies from it.

<p align="center">
<img src="https://i.imgur.com/hqBvyEB.png" height="80%" width="80%" alt="img"/>
</p>

To join the domain, right-click the Start Menu and click "System".

<p align="center">
<img src="https://i.imgur.com/LLmJxVx.png" height="80%" width="80%" alt="img"/>
</p>

Click "Rename this PC (Advanced)" > "Change". Select "Domain", type "mydomain.com" and click "Ok.

We didn't get an error message like we got earlier. Instaed it's prompting us for our username and password.

Type "mydomain.com\jane_admin" as username, and "Password1" as password. Click "Ok".

<p align="center">
<img src="https://i.imgur.com/WWLbfVX.png" height="80%" width="80%" alt="img"/>
</p>

A new window will pop up. Click "Ok" > "Ok

<p align="center">
<img src="https://i.imgur.com/kvdQPZQ.png" height="80%" width="80%" alt="img"/>
</p>

Click "Restart Now". You will be disconnected from Client-1's VM.

Go back to virtual machines in Azure, click Client-1, and copy it's public IP address.

<p align="center">
<img src="https://i.imgur.com/ozCVl6k.png" height="80%" width="80%" alt="img"/>
</p>

Open Remote Desktop, paste Client-1's public IP address, and click "Connect".

<p align="center">
<img src="https://i.imgur.com/i5beMA2.png" height="80%" width="80%" alt="img"/>
</p>

Instead of logging in as "labuser", click "More choices" > "Use a different account". Type "mydomain.com\jane_admin" as username and "Password1" as password.

<p align="center">
<img src="https://i.imgur.com/p6Qb1ny.png" height="80%" width="80%" alt="img"/>
</p>

Click "Yes".

<p align="center">
<img src="https://i.imgur.com/PjQa0Ka.png" height="80%" width="80%" alt="img"/>
</p>

Open command prompt, and run the command "hostname" and "whoami".

As you can see, we are able to log into Client-1's VM as jane doe (jane_admin), even though we've never logged into Client-1 before as jane doe (jane_admin)

This is because Client-1 is a member of the domain (DC-1) and jane_admin is an admin account within the domain.

<p align="center">
<img src="https://i.imgur.com/hqBvyEB.png" height="80%" width="80%" alt="img"/>
</p>

Next, we will setup a remote deskstop for non-administrative users on Client-1.

Right-click the Start Menu and click "System".

<p align="center">
<img src="https://i.imgur.com/5hpVowS.png" height="80%" width="80%" alt="img"/>
</p>

Click "Remote Desktop".

<p align="center">
<img src="https://i.imgur.com/pf2A6AK.png" height="80%" width="80%" alt="img"/>
</p>

Click "Select users that can remotely access this PC".

<p align="center">
<img src="https://i.imgur.com/2fhQulJ.png" height="80%" width="80%" alt="img"/>
</p>

Click "Add", type "Domain Users" in the box, click "Check Names", and click "Ok" > "Ok".

This will allow all domain users and non-adminstrative users to log into Client-1's VM

Minimize Client-1's VM and open DC-1's VM.

<p align="center">
<img src="https://i.imgur.com/xB40N1G.png" height="80%" width="80%" alt="img"/>
</p>

In DC-1, click the Start Menu, collapse "Windows Administrative Tools", and click "Active Directory Users and Computers".

<p align="center">
<img src="https://i.imgur.com/vfiOL30.png" height="80%" width="80%" alt="img"/>
</p>

Collapse "mydomain.com" and click "Users". You can see that "Domain Users" are the group we've just given access to Client-1 VM.

Double-click "Domain Users" and click "Members". "Members" contain all the users in the "Domain Users" group, as shown in the image above. Anyone in this group is allowed to log into Client-1's VM.

<p align="center">
<img src="https://i.imgur.com/hDlQlne.png" height="80%" width="80%" alt="img"/>
</p>

We will now create a bunch of additional users and attempt to log into Client-1 with one of the users.

In DC-1, search for "powershell_ise" and right-click "Windows PowerShell ISE", and click "Run as administrator", as shown in the image above.

<p align="center">
<img src="https://i.imgur.com/EhnNQQO.png" height="80%" width="80%" alt="img"/>
</p>

Copy the script found in this repository: https://github.com/CollinsU99/Generate-Names-Create-Users.ps1.git and paste it in the "powershell ise" new file. 

In PowerShell Ise, click the "create new file" icon located in the upper left corner, paste the script you copied in the new file, and click the green play button, as shown in the image above.

NOTE: The script will create 10,000 random accounts in the "_EMPLOYEES" organizational unit in AD, and they will all have "Password1" as their password.

<p align="center">
<img src="https://i.imgur.com/3GIuUtV.png" height="80%" width="80%" alt="img"/>
</p>

As shown in the image above, the accounts are being created.

<p align="center">
<img src="https://i.imgur.com/LL3lWox.png" height="80%" width="80%" alt="img"/>
</p>

In Active Directory, right-click "_EMPLOYEES" and click "Refresh", and you will see a bunch of random accounts have been created.

<p align="center">
<img src="https://i.imgur.com/jJ3I3d2.png" height="80%" width="80%" alt="img"/>
</p>

Double-click on any random name, click "Account", and copy the username of the account, and minimize DC-1

We will use this username to log into Client-1 VM.

<p align="center">
<img src="https://i.imgur.com/wbQ6NWt.png" height="80%" width="80%" alt="img"/>
</p>

Open Client-1 VM, Click the Start Menu, click "jane doe", and click "Sign Out".

<p align="center">
<img src="https://i.imgur.com/7XcnGk6.png" height="80%" width="80%" alt="img"/>
</p>

In Azure portal, go to "virtual machines", click Client-1 VM, and copy it's public IP address.

<p align="center">
<img src="https://i.imgur.com/hcYAoBU.png" height="80%" width="80%" alt="img"/>
</p>

Open Remote Desktop, paste Client-1's public IP address, and click "Connect".

Click "More choices" > "Use a different account", type the random username we created, and type "Password1" in the password box. Then, click "Ok".

<p align="center">
<img src="https://i.imgur.com/eXQPCSI.png" height="80%" width="80%" alt="img"/>
</p>

We are now connecting to one of the random accounts we created.

NOTE: The account "bag.pilufa" has never logged into Client-1 before but is able to do so because it has been created as a user in the Domain Controller (DC-1).

<p align="center">
<img src="https://i.imgur.com/fY3GPxW.png" height="80%" width="80%" alt="img"/>
</p>

To confirm, open command prompt in Client-1. run the commands "hostname" and "whoami".

As shown in the image above, "bag.pilufa" is logged in as a user in Client-1.

<p align="center">
<img src="https://i.imgur.com/5b5c8HP.png" height="80%" width="80%" alt="img"/>
</p>

Click the pinned File Explorer app on the taskbar. Click "This PC" and double-click the C drive.

<p align="center">
<img src="https://i.imgur.com/hawpjni.png" height="80%" width="80%" alt="img"/>
</p>

Click the "Users" folder.

<p align="center">
<img src="https://i.imgur.com/CyHx22J.png" height="80%" width="80%" alt="img"/>
</p>

Anytime a new user logs into Client-1, a new folder will be created.

As shown in the image above, the folders "labuser", "jane_admin", and "bag.pilufa" were created because we have logged in to Client-1 with those accounts before.
