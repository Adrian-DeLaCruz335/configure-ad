<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines. <p></p>
Active Directory is a software built and maintained by Microsoft that centrally manage thousands of user accounts in a single place. This allows you to manage devices on a large scale.
<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: Set up the Azure environment for Active Directory

- Step 2: Install and configure Active Directory services

- Step 3: Add client computers to the domain

- Step 4: Enable and configure Remote Desktop connectivity

- Step 5: Create and organize Organizational Units (OUs)

- Step 6: Use PowerShell to create multiple user accounts

<h2>Setting up and configuring Azure Virtual Machines for Active Directory</h2>

<p>
<img width="836" height="557" alt="image" src="https://github.com/user-attachments/assets/a3c4a8fb-d8d3-4ba3-a76a-72b9d38a874e" />
</p>
<p>
I deployed a virtual machine called DC-1 to serve as the domain controller in the Active Directory environment. It manages user accounts, authentication processes, and domain security policies. DC-1 also provides DNS services, which allows the client VM (Client-1) to successfully discover and join the domain.
</p>
<br />

<p>
<img width="837" height="776" alt="image" src="https://github.com/user-attachments/assets/94c5fab5-899f-4bf9-b777-37e5d0af4145" />
</p>
<p>
I made sure the virtual machine was connected to the same vNet. This virtual network creates a private environment in Azure that allows DC-1 and Client-1 to communicate securely. Being on the same network allows Active Directory tasks like domain joining to work correctly, similar to a real office network.
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/1cf39314-a0f1-4677-b83c-7fedb7af41de" />
</p>
<p>
I then created "Client-1", a virtual machine on the same virtual network. This VM acts as a regular domain-connected workstation. It allows me to join the domain managed by DC-1, log in with domain users, and test authentication and policy settings.
</p>
<br />

<p>
<img width="861" height="980" alt="image" src="https://github.com/user-attachments/assets/50db8484-2ac4-40a4-837f-41c01e2edc98" />


</p>
<p>
The private IP address of DC-1 was changed from dynamic to static to ensure it remains consistent across reboots. This is critical because client systems rely on a stable IP address to locate DC-1 for DNS services and domain authentication. Without a static address, changes to the IP could disrupt connectivity, name resolution, and domain join processes. Assigning a static IP guarantees dependable communication within the lab network.
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/2e08329f-f47f-4181-ace9-19de670e7c3d" />
</p>
<p>
Next, I configured the DNS settings on Client-1 to point to DC-1. Because DC-1 hosts both Active Directory and DNS services, this step enables Client-1 to resolve the domain and successfully join it. Without using DC-1 for DNS, the client would not be able to find the domain controller.
</p>
<br />

<p>
  <img width="839" height="497" alt="image" src="https://github.com/user-attachments/assets/d909d417-0951-4032-b942-778d8c011195" />

<img width="1133" height="917" alt="image" src="https://github.com/user-attachments/assets/3a1677fc-3663-4a06-b33f-4bb8ea7789a8" />

/>

</p>
<p>
I logged into Client-1 to test the setup by pinging DC-1’s private IP, confirming network connectivity. I then checked the DNS settings using ipconfig to make sure Client-1 was pointing to DC-1 as its DNS server so it could find the domain.</p>
</p>
<br />

<h2>Setting up Active Directory to add admins and users.</h2>

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/4ce98c8a-da61-45dd-9102-a31903bb6f2d" />
</p>
<p>
To install Active Directory, I logged into DC-1 and launched Server Manager from the Start menu. From there, I selected Add Roles and Features, advanced through the prompts, and installed Active Directory Domain Services when prompted to choose server role
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/bb621a6a-5b06-4803-ba24-fa93b5fa4686" />
</p>
<p>
To promote DC-1 to a domain controller, I opened Server Manager and clicked the notification flag indicating the server was ready for promotion. I selected Add a new forest, specified mydomain.com as the domain name, and configured the Directory Services Restore Mode password. After finishing the wizard, the server was restarted. I then logged back in using the domain account mydomain.com\labuser.
</p>
<br />

<p>
<img width="400" alt="image" src="https://github.com/user-attachments/assets/deb2a9c5-adbc-4646-bb69-7a1f2c9be582" /> <img width="400" alt="image" src="https://github.com/user-attachments/assets/931107ca-0fbc-494f-8411-5238e2bbcae3" />
<img width="400" alt="image" src="https://github.com/user-attachments/assets/42559932-4371-45a0-8d9f-e11801156cd1" />
</p>
<p>
The next step was creating a Domain Admin account within the Active Directory environment. This account provides full administrative control over the domain, allowing management of users, computers, security settings, group policies, and permissions.

To begin, I opened Active Directory Users and Computers from the Windows menu by navigating to Start → Administrative Tools → Active Directory Users and Computers.

I then created an organizational unit named _EMPLOYEES by right-clicking the domain (mydomain.com) in the left pane, selecting New → Organizational Unit, entering _EMPLOYEES, and clicking OK.
The same process was repeated to create a second organizational unit named _ADMINS.
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/c734688d-da52-4845-825b-a57395b24d53" />
</p>
<p>
Next, I created a new user named Jane Doe to grant full administrative privileges for domain management.
In Active Directory Users and Computers (ADUC), I right-clicked the _ADMINS organizational unit, selected New → User, entered the full name Jane Doe and username jane_admin, set a password, and completed the setup.
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/0e68de8f-049f-4c56-980c-34d86d3808b4" />
</p>
<p>
<ins>Then, add jane_admin to the “Domain Admins” group: </ins>

To grant Jane Doe full administrative rights, I right-clicked the jane_admin account and selected Properties. In the Member Of tab, I clicked Add, searched for Domain Admins, and added the account to that group.
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/86c3b8a1-e5bf-45eb-8db8-58ebc8b2a8da" /> 
</p>
<p>
I logged into Client-1 as a local administrator and joined it to the domain.
To do this, I right-clicked Start, selected System, and then clicked Rename this PC (Advanced) → Change. Under Member of, I selected Domain and entered mydomain.com. I provided the credentials of the domain admin account (mydomain.com\jane_admin) and restarted the VM when prompted.

Thanks to the DNS configuration pointing Client-1 to DC-1, the client was able to locate the domain controller and successfully join mydomain.com.
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/b4823c58-c32e-4ccb-887b-7f05bef446f8" />
</p>
<p>
Then go back to DC-1 and open Active Directory Users and Computers. </p>
Look under the Computers container to confirm "Client-1" is listed. </p>
Create a "_CLIENTS" OU and move Client-1. </p>

**Why create a "_CLIENT" OU?** </p>
The _CLIENTS OU is used to keep clients together and to make management easier so that Group Policies (GPOs) can be applied specifically to the _CLIENTS OU, targeting only client machines. </p>
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/2a70dc62-ac11-44d6-8d01-24f365918625" />
</p>
<p>
Now I'm going to setup Remote Desktop for non-administrative users on Client-1. </p>
This is because I want to allow regular domain users (not just admins) to remotely log into Client-1.</p>
  This is to simulate a real-world IT environment where employees will need remote access to their workstations and to allow for testing of user logins and remote support scenarios. </p> So if a client needs remote technical support,  instead of telling the user what to click and where to go, this will allow admins to remote into their workstation to provide easier technical support and troubleshooting. </p>
</p>
<br />

<p>
<img width="1275" height="857" alt="image" src="https://github.com/user-attachments/assets/49e526e0-0d72-45a1-8485-3e7a56f0b404" />
</p>
<p>
<ins>Next, I'm going to create a bunch of additional users and attempt to log into client-1 with one of the users.</ins>
  
   - Log in to DC-1 and open PowerShell_ise as an administrator:
        - Search for PowerShell ISE.
        - Right-click and select Run as administrator to open it with elevated privileges.
   - Create a new file and paste the script:
        - In PowerShell ISE, go to File > New to create a new script file.
        - Copy the provided [PowerShell script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) and paste it into the new file.
Now if I go to ADUC and navigate to _EMPLOYEES OU, I can see the newly created user accounts.
</p>
<br />

In the next section of the project, I will configure Group Policies for testing and simulating password resets, account lockouts, and user permissions for Help Desk practices. </p>
