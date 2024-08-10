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
- Ensure Connectivity Between the Client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in Active Directory
- Join Client-1 to Domain
- Setup Remote Desktop for Non-Administrative Users on Client-1
- Create Additional Users and Log into Client-1 With a User

<h2>Deployment and Configuration Steps</h2>
<h3>Setup Resources in Azure</h3>
<p><h4>Create a Virtual Machine<p></h4>
  <ul>
<li>Resouce Group Name: AD-Lab</li>
  <li>Virtual Machine Nmae: DC-1</li>
  <li>Image: Windows Server 2022</li>
  <li>Choose a username and password. Take note of the Resource Group and Virtual Network (Vnet) that get created at this time</li>
  <li>Click review and create</li>
  </ul>
<img width="766" alt="ActiveDir_ResourceGroup" src="https://github.com/user-attachments/assets/7905049b-9082-4567-b31d-9c8deaf051fd">

<p><h4>Set Domain Controller's NIC Private IP address to be static<p></h4>
<ul>
<li>Once the VM deployment is complete, click Network Settings within your virtual machine</li>
<li>Click the IP Configuration link</li>
<li>Take note of DC-1's Private IP address</li> 
<ul>
<p>
<img width="673" alt="ActiveDir_ipConfig" src="https://github.com/user-attachments/assets/f5cff329-e305-4d28-8dbd-9c3859b2aadc">
<ul>
<li>Click ipconfig, choose Static and save</li>
</ul>
<p>
<img width="825" alt="ActiveDir_Static" src="https://github.com/user-attachments/assets/bcbdc985-2f19-4434-97b5-9b32b834909d">
<p><h4></h4>Create the Client VM (Windows 10)</p><h4></h4>
<ul>
<li>Create Virtual Machine and use the same Resource Group and Vnet that was created in Step 1.</li>
<li>Virtual Machine Name: Client-1</li>
<li>Image: Windows 10</li>
<li>Choose a username and password.</li>
<li>Create</li>
</ul>
<h3>Ensure Connectivity Between the Client and Domain Controller</h3>
<p><h4>Copy Client-1's public IP and use it to login to Client-1 with Remote Desktop</p></h4>
<img width="477" alt="ActiveDir_Login1" src="https://github.com/user-attachments/assets/5650ec05-c5d2-4efa-9fdd-eaa7e4c76b0a">
<p><h4>In Client-1 VM, type cmd in the search box and open Command Prompt<p></h4>
<ul>
<li>Inside Command Prompt, ping DC-1’s private IP address with ping -t (ip address) It will give a Request Timeout reply
</li>
<p><h4>Login to DC-1 with Remote Desktop</p></h4>
<ul>  
<li>In the search box, type firewall and choose Windows Defender and Firewall Advanced</li>
</ul>
<p></p> 
<img width="481" alt="ActiveDir_Firewall" src="https://github.com/user-attachments/assets/e5d28046-f659-4546-b9d7-3dac0d82ad49">
</p>
<ul>
<li>Select Inbound Rules</li>
<li>Sort the Protocol column for ICMP, click and enable the ICMP Echo Request rules</li>
</ul>
<p></p>
<img width="1270" alt="ActiveDir_EnablePing" src="https://github.com/user-attachments/assets/96210789-1cca-40d9-a8e5-928bb1bb315e">
<p>
<ul>
<li>Check back to Client-1 to see the ping succeed</li>
</ul>
<p></p>
<img width="370" alt="ActIveDir_PingReply" src="https://github.com/user-attachments/assets/51fd5ad0-4e07-4d09-a4bb-eb5523594ff0">
<h3>Install Active Directory</h3>
<p><h4>Inside DC-1, choose Add Roles and Features to begin installing Active Directory</p></h4>
<img width="345" alt="ActiveDir_AddFeatures" src="https://github.com/user-attachments/assets/ee9d8831-0e93-42c8-869f-a97ae763ffb8">
<p></p>
<ul>
<li>Click next until you get to the Select Server Roles Menu, check the Active Directory Domain Services Box then Add Features</li>
</ul>
<p></p>
<img width="278" alt="ActiveDir_SelectRoles" src="https://github.com/user-attachments/assets/c6146834-b879-49a2-ad30-691d18dab591">
<ul>
<li>Click next and install</li>
<li>Once installation is complete, click the yellow notification symbol and "Promote this Server" link</li>
</ul>
<p></p>
<img width="363" alt="ActiveDir_PromoteServer" src="https://github.com/user-attachments/assets/bdc1480d-e66a-4a48-97f3-4262c9527e2d">
<ul>
<li>In Deployment Configuration Menu, select Add New Forest and enter a domain name (can be anything, just remember what it is)</li>
<p>
<img width="646" alt="ActiveDir_Forest" src="https://github.com/user-attachments/assets/7529c6fd-5966-4dd9-a8fb-4e795506ad93">
</p>
<ul>
<li> Click Next -> create a password -> Next</li>
<li>When the Netbios Domain Name box populates, choose next and install</li>
<li>The VM will restart then log back into DC-1 as user: whateveryourdomainis.com\labuser</li>
</ul>
<h3>Create an Admin and Normal User Account in AD</h3>
<p><h4>In the Search box, search for Active Directory Users and Computers (ADUC)</p></h4>
<img width="397" alt="ActiveDir_Users1" src="https://github.com/user-attachments/assets/73b723ee-decf-446e-b6f5-468e7096f25b">
<ul>
<li>Select your domain name, right-click and create an Organizational Unit</li>
<li>Name the Organizational Unit, "_EMPLOYEES"</li>
<li>Create a new Organizational Unit (OU) named "_ADMINS"</li>
</ul>
<p></p>
<img width="604" alt="ActiveDir_OU" src="https://github.com/user-attachments/assets/92a44fde-92be-4cb1-a9b7-99ccbaaee237">
<ul>
<li>In the _ADMINS folder, create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
</li>  
</ul>
<p></p>
<img width="545" alt="ActiveDir_Employee" src="https://github.com/user-attachments/assets/b04f71c5-b7a3-4b59-a023-72c019a6b452">
<ul>
<p>Uncheck "change password at login" and check "password never expires" -> Next -> Finish
<ul>
<li>Right click Jane Doe -> Properties -> Member of -> Add</li>
<li>Type "domain" in the text box and select check names. Choose "Domain Admins" -> Ok -> Apply -> Ok </li>
</ul>
<img width="558" alt="ActiveDir_Domain" src="https://github.com/user-attachments/assets/59877d38-9586-43c1-a0a0-da43e1304817">
<ul>
<li>Log out/close the Remote Desktop connection to DC-1 and log back in as “whateveryourdomainis.com\jane_admin”</li>
<li>Use jane_admin as your admin account from now on</li>
</ul>
<h3>Join Client-1 to your domain</h3>
<p><h4>From the Azure Portal, set Client-1's DNS settings to the DC's Private IP address</p></h4>
<ul>
<li>In the Azure Portal, go to Virtual Machines -> DC-1 -> Take note of the Private IP address</li>
<img width="246" alt="ActiveDir_PrivateIP" src="https://github.com/user-attachments/assets/ed8a6d3e-96c7-4b4f-ab8f-3d6a285a3ec5">
</ul>
<ul>
<li>In the Azure Portal, go to Virtual Machines -> Client-1 -> Networking -> Network Interface</li>
</ul>
<img width="709" alt="ActiveDir_Client1Nic" src="https://github.com/user-attachments/assets/ed8d4090-4e34-48ca-ba12-63732984e7ab">
<p>
<ul>
<li>Setting -> DNS Server -> Custom -> Enter DC-1's Private IP Address in DNS Server box</li> 
</ul>
<img width="584" alt="ActiveDir_DNS" src="https://github.com/user-attachments/assets/c8beb7d1-2f9f-4bf1-aa21-5e211ae99481">
<p>
<ul>
<li> Go back to Client-1 and select Restart</li>
</ul>
<img width="592" alt="ActiveDir_Restart" src="https://github.com/user-attachments/assets/e54f782a-f336-439c-a150-fd260e3d9e8f">
<p><h4>Log back in to Client-1 (Remote Desktop) with the original credentials and join it to the domain (computer will restart)
</p></h4>
<ul>
<li>Log back in to Client-1 via Remote Desktop</li>  
<li>Right-click the start menu -> System -> Rename this PC -> Change</li>
<li>Select Domain and add your domain name</li>
</ul>
<img width="317" alt="ActiveDir_ChngDomain" src="https://github.com/user-attachments/assets/52f9c414-d01b-4f6b-82b9-c949aa275faf">
<p>
<ul>
<li>Enter the admin login credentials and password (whateveryourdomainis.com\jane_admin)/li>
<li>A confirmation screen should pop up welcoming you to the new domain and the VM will restart. You may have to log back via Remote Desktop. 
</ul>
<img width="279" alt="ActiveDir_WelcomDomain" src="https://github.com/user-attachments/assets/a5c361a8-b181-41e9-9c9b-cb09e2e1bbc2">
<h3>Setup Remote Desktop for Non-administrative Users on Client-1</h3>
<p>
<ul>
<li>Log into Client-1 as "whateveryourdomainis.com\jane_admin" and right-click the start menu</li>
<li> Select System -> Remote Desktop -> Select users that can remotely access this PC</li>
<li>Select Add -> Enter "Domain" in the text box -> Click Check Names</li>
</ul>
<img width="461" alt="ActiveDir_Domain Users" src="https://github.com/user-attachments/assets/54d95953-99bd-42ca-8fc1-7d5e32f328f6">
</p>
<ul>
<li>Select Domain Users -> Ok
</li>
</ul>
<h3>Create Additional Users and Log Into Client-1 With One of the Users</h3>
<p>
<ul>
<li>Log in to DC-1 as jane_admin, if not already logged in</li>
<li>Go to Powershell ISE in the search bar and run as administrator</li>
<img width="400" alt="ActiveDir_PowerShell" src="https://github.com/user-attachments/assets/90320ecb-18bc-40a9-955d-ae99c928bbca">
<p>
Create a new File and paste the contents of the script into it
  <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">PowerShell Script</a>
</p>
<li>Run the script and observe the accounts being created</li>
<p>
<img width="780" alt="ActiveDir_PowerShell Script" src="https://github.com/user-attachments/assets/631d4064-a15c-4b16-85f0-0d54c07d67b8">

<li>When finished, open ADUC and observe the accounts in the appropriate OU</li>
</p>![Uploading ActiveDir_EmployeeUpload.png…]()
<p>
<li>Attempt to log into Client-1 with one of the accounts. (Take note of the password in the script)</li>
</p>
<img width="450" alt="ActiveDir_NonAdminLogin" src="https://github.com/user-attachments/assets/b8d1891f-ceba-4e72-a63f-f9d2e064b6bc">
