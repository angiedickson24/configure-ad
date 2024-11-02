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

- Step 1: Install Active Directory
- Step 2: Create a Domain Admin user for the domain
- Step 3: Join client-1 to domain

<h2>Deployment and Configuration Steps</h2>

<p>
Before deploying Active Directory, we need to set up two VMs on Azure: one Windows Server for the domain controller (named DC-1) and one Windows Pro machine as a client (named Client-1). It's important to ensure both machines are on the same virtual network.

Once that's complete, we’ll log into DC-1.
</p>
<br />
<p>
<img src="https://github.com/user-attachments/assets/c769dda7-160a-463f-a999-10ec6e3a52e3" height="80%" width="80%" alt="server manager in DC-1"/>
</p>
<p>
The Server Manager should load automatically; if it doesn’t, click Start to find the Server Manager icon. Select Add roles and features and accept the defaults until you reach Select Server Roles. At that point, choose Active Directory Domain Services.
</p>
<br />
<p>
<img src="https://github.com/user-attachments/assets/850f1f4f-e318-464c-9950-f0f40d7aaa9c" height="80%" width="80%" alt="installing AD"/>
</p>
<p>
Proceed by checking Restart the destination server automatically if required, then click Install. Once the installation is complete, return to Server Manager and click the flagged icon in the top right corner. Next, click the link that says Promote this server to a domain controller.
</p>
<br />
<p>
<img src="https://github.com/user-attachments/assets/1ee98876-3593-4ec9-b4bf-1e20b455b15c" height="80%" width="80%" alt="promote to domain controller"/>
</p>
<p>
In the deployment configuration, select Add a new forest and enter mydomain.com. You can choose any domain name you prefer in practice.
</p>
<br />
<p>
<img src="https://github.com/user-attachments/assets/d17d8b12-80cd-43b4-bd0a-76bdd292ca00" height="80%" width="80%" alt="add new forest mydomain"/>
</p>
<p>
Click Next, and when prompted to enter the Directory Services Restore Mode password, you can choose any password since it's unlikely to be used. Continue with the configuration, and you will be automatically signed out as Active Directory completes the installation.

Next, we’ll log into the Client-1 machine. If you try logging in with your usual credentials, it will fail because no domain is specified, as we've just set up DC-1 as the domain controller. To log in, you need to specify the domain like this: mydomain.com\your username, and then enter the password.

Once logged in, return to the DC-1 machine. The next step is to create a Domain Admin user for managing the entire domain, which is a crucial task. Click on Start -> Windows Administrative Tools -> Active Directory Users and Computers. Then, right-click on mydomain.com -> New -> Organizational Unit (OU).
</p>
<br />
<p>
<img src="https://github.com/user-attachments/assets/27d9f0b0-265c-4a1d-aff2-1fc6183e1664" height="80%" width="80%" alt="create OU for domain"/>
</p>
<p>
We can name the new OU _EMPLOYEES and create another one called _ADMINS. Next, we’ll create an admin user. Right-click on _ADMINS -> New -> User. Name this admin user Jane Doe, with the username jane_admin, and set a password of your choice.

Even though Jane Doe is in the _ADMINS OU, she isn't a Domain Admin yet because we haven't assigned her those privileges. To do this, click on _ADMINS to find Jane Doe. Right-click on her, select Properties, then go to the Member Of tab. Type Domain Admins in the box, click Check Names, then click OK and apply the settings.
</p>
<br />
<p>
<img src="https://github.com/user-attachments/assets/09c0899a-a5c6-42b8-be70-83021c9dccc9" height="80%" width="80%" alt="set Jane as Domain admin"/>
</p>
<br />
Next, we need to change the DNS settings on Client-1 to point to the private IP address of DC-1.

<p>
<img src="https://github.com/user-attachments/assets/5bc33311-7037-4a64-ac19-6d11909e6ce2" height="80%" width="80%" alt="preconfigure vms before deploy AD"/>
</p>
<br />
<p>
Log back into Client-1 as Jane, remembering to use *mydomain.com* before the username. Right-click on the Start menu and select Settings. Then go to Rename this PC (advanced). Under Computer Name, click Change, enter mydomain.com in the Domain field, and click OK.
</p>

<p>
<img src="https://github.com/user-attachments/assets/b847bdfa-2562-48b4-be8d-3fc4304810f1" height="80%" width="80%" alt="join client-1 to domain"/>
<img src="https://github.com/user-attachments/assets/af138c07-f43f-490c-bc8a-3199ba175601" height="80%" width="80%" alt="client joined domain"/>
</p>
<p>You will need to restart Client-1 for these changes to take effect. After restarting, return to DC-1 to verify if Client-1 has joined the domain. In Active Directory Users and Computers, click on mydomain.com and then on Computers; you should see Client-1 listed there.
</p>
<br />

<p>
  <img src="https://github.com/user-attachments/assets/1344ee78-88e2-49e9-8496-933bd354572d" height="80%" width="80%" alt="confirm client 1 in domain"/>
</p>
<p>

</p>
