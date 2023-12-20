<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Installing Active Directory in the Cloud (Azure)</h1>
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
Before using the VMs, it is important to set the domain controller VM's IP address as static. 
</p>  

![AD-Lab 5](https://github.com/Jacob-Oq/install-ad/assets/150084528/6dbd3c8e-8508-43b7-a02c-6a709e88a81c)

<p>
The VMs will not be able to communicate with each other if both have dynamic IPs despite being on the same vnet. If this is not done, the client will not be able to join the domain which we will create shortly. On the Azure portal, click on the Networking tab on the domain controller VM. Click on the Network Interface and open the IP configurations tab. Find "ipconfig1" below "IP Settings" and "Name" then click it to open the "Edit IP configuration" window. Toggle the Assignment switch or click in the "radio" input type (depending on your OS) to Static and save your changes. The domain controller must have a static IP and it will be used as a reference when we make configurations. 
</p>

<br />
<p>
Now let's log in to the client VM and see if there is connectivity to the domain controller. Using "ping -t" and the domain controller private ip address, the connection will show that it is being timed out. 
</p>

![AD-Lab 10](https://github.com/Jacob-Oq/install-ad/assets/150084528/11eca34b-7908-40fc-b586-470dd17384a5)

![AD-Lab 11](https://github.com/Jacob-Oq/install-ad/assets/150084528/1c1cf08c-7b6b-47d0-8ada-7f430b4d1141)

<p>  
On the domain controller VM, we need to enable ICMPv4 on the local Windows Firewall. Within the search bar, type wf.msc to open Windows Defender Firewall. 
</p>

![AD-Lab 12](https://github.com/Jacob-Oq/install-ad/assets/150084528/d0cebc5c-654f-4dca-848f-df3e626a1e64)

<p>
Click on Inbound Rules and enable the Core Networking Diagnostics - ICMP Echo Request rules.
</p>

![AD-Lab 13](https://github.com/Jacob-Oq/install-ad/assets/150084528/4f739e15-b4d4-444c-9c88-511410de44dc)

![AD-Lab 14](https://github.com/Jacob-Oq/install-ad/assets/150084528/1882bb36-df4b-4827-b772-5b325b2990f9)


<p>Returning to the client VM will show that the ping is now resolving without errors. </p>

![AD-Lab 15](https://github.com/Jacob-Oq/install-ad/assets/150084528/090aca3b-17de-45da-85cf-ea1ad88ab123)


<br />
<p>
Now we can install Active Directory on the domain controller VM. With Server Manager open, click on Add Roles and Features and click Next. 
</p>

![AD-Lab 16](https://github.com/Jacob-Oq/install-ad/assets/150084528/190a4dd2-60bb-497d-a73b-13c3fc9a6f4a)

  
<p>Confirm the private IP address of the domain controller VM.</p> 

![AD-Lab 17](https://github.com/Jacob-Oq/install-ad/assets/150084528/0ba2d2cc-f3fa-4ab1-b328-34ed52a50e75)


<p>In the Server Roles tab, click on Active Directory Domain Services.</p> 

![AD-Lab 18](https://github.com/Jacob-Oq/install-ad/assets/150084528/9eb01cee-06c2-4af7-b7c3-63bf07288297)


<p>Click Add Features, click Next, then Install. Once it's installed click "close" at the bottom.</p> 

![AD-Lab 19](https://github.com/Jacob-Oq/install-ad/assets/150084528/76f87a57-f132-4eeb-8a48-9a6a537de76b)

![AD-Lab 21](https://github.com/Jacob-Oq/install-ad/assets/150084528/ef0edb52-533f-4139-9fe4-44ede9a81c1f)


<p>
Next we have to promote the server into a domain controller. In Server Manager, there is a warning sign in the top right corner under a yellow flag. Click on that flag and click Promote this server to a domain controller. Click on Add a new forest and specify a domain name.
</p>

![AD-Lab 22](https://github.com/Jacob-Oq/install-ad/assets/150084528/8bcfd579-3158-443a-bd18-4f398182109f)

![AD-Lab 23](https://github.com/Jacob-Oq/install-ad/assets/150084528/7e969d6b-6eba-4284-83fc-39e9bccc4d0d)


<p>Specify a password for the domain and click on Next on each screen and Install.</p>

![AD-Lab 24](https://github.com/Jacob-Oq/install-ad/assets/150084528/7d1a48b8-1769-4fa2-ad95-48337101d6b4)

<br />

<h2>An Important Note </h2>
<p>
When logging back in to the domain controller VM through Remote Desktop Connection, it is important to log in with the context of the domain. Type out the domain path and then the name of the user. For example: mydomain.com\labuser. In the example below you will see if this is not done correctly, you will not be able to access the domain controller and will have to retype your credentials.
</p>

![AD-Lab 25](https://github.com/Jacob-Oq/install-ad/assets/150084528/a2185d0e-b621-4496-aad2-7da7084a9657)

<p>
Now that Active Directory is installed, configurations can be implemented and the client VM will be able to join the domain that was created.
</p>


