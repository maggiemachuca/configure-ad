<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (22H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

## Step 1: Install Active Directory Domain Services on dc-1

- Navigate to **Server Manager**
- Click on **Add roles and features**
- Once you click this we will be going through the installation wizard
  
<img width="623" height="335" alt="Screenshot 2025-09-24 at 3 00 37 PM" src="https://github.com/user-attachments/assets/16339968-7ecc-4b39-8b2a-ba686f61b60c" />

- On Server Selection, choose the domain controller.
- In this case it is **dc-1**

<img width="786" height="560" alt="Screenshot 2025-09-24 at 3 02 25 PM" src="https://github.com/user-attachments/assets/a0df264b-86e1-497d-97f4-f06d05242a45" />

- On Server Roles, choose **Active Directory Domain Services**
- After clicking that this window pops up (screenshot below)
- Click **Add Features**

<img width="415" height="433" alt="Screenshot 2025-09-24 at 3 04 09 PM" src="https://github.com/user-attachments/assets/c1db0782-2144-41f6-b3b2-11b842c64260" />

- On Features, click next. There's no need to select anything else here
- On AD DS, click next.
- On Confirmation, checkmark the **Restart** option (screenshot below)
- This is to ensure that the VM has no problems installing Active Directory (check later)
- Click install

<img width="783" height="557" alt="Screenshot 2025-09-24 at 3 06 40 PM" src="https://github.com/user-attachments/assets/c1e81c8a-f445-4406-b5db-ce3523113e92" />


- Now that we've installed Active Directory, we have to setup this VM to become our domain controller.
- Let's set up the **dc-1** VM in a new Active Directory forest.

- On **Server Manager**, click the flag icon and then click **Promote this server to a domain controller** (screenshot below)
  
<img width="390" height="194" alt="Screenshot 2025-09-24 at 3 13 35 PM" src="https://github.com/user-attachments/assets/695d73b6-3c6c-4ea5-98f1-27db17d871dd" />

- Click **Add a new forest**
- In the root domain name, type: **mydomain.com**
- Click Next.
  
<img width="755" height="309" alt="Screenshot 2025-09-24 at 3 15 23 PM" src="https://github.com/user-attachments/assets/b27c8044-edd3-47de-9a44-95fdad6fc4cd" />

- Set your password in the next section
- Click Next.

<img width="465" height="98" alt="Screenshot 2025-09-24 at 3 17 20 PM" src="https://github.com/user-attachments/assets/309f4a7e-0dae-4173-9df7-75ad425f465f" />

- In DNS options, uncheck **Create DNS delegation** (why?)
- Click next.

- In additional options, you can leave the **NetBIOS domain name** alone.
- Click next.

- In paths, you can leave this part alone.
- Click next.

- In Review options, you can leave this part alone.
- Click next.

- You should end up at this screen (screenshot below)
- Go ahead and click Install.

<img width="756" height="556" alt="Screenshot 2025-09-24 at 3 21 07 PM" src="https://github.com/user-attachments/assets/dada4866-6d5e-4985-b9ed-7d38f6b8dcff" />

- Your VM should automatically restart after installation is complete.

- Since we have established our VM as a domain controller, we now have to login using the appropriate domain.
- Let's try logging back into our domain controller.

<img width="434" height="230" alt="Screenshot 2025-09-24 at 3 26 04 PM" src="https://github.com/user-attachments/assets/897b25e4-f588-4309-8500-be380ef8fce1" />


## Step 2: Create a Domain Admin user within the domain

- Once you are logged in, Click the Windows icon
- Click on **Windows Administrative Tools** -> **Active Directory Users and Computers**
- This window should pop up. (screenshot below)

<img width="751" height="527" alt="Screenshot 2025-09-24 at 3 33 10 PM" src="https://github.com/user-attachments/assets/d104b6b4-e3ea-448d-a099-3e632411fd97" />

- We are going to create two **Organizational Units**
- The first one will be called **_EMPLOYEES**
- The second one will be called **_ADMINS**
- I personally like to put an underscore before writing the OU names so we can differentiate them easily from the other items.

- Right click on **mydomain.com**
- Click New -> Organizational Unit

<img width="755" height="533" alt="Screenshot 2025-09-24 at 3 37 06 PM" src="https://github.com/user-attachments/assets/f52ee86f-2f72-4baf-9800-86e0e3e3fa75" />

- Name this OU **_EMPLOYEES**

<img width="429" height="376" alt="Screenshot 2025-09-24 at 3 38 09 PM" src="https://github.com/user-attachments/assets/3118f88a-7053-4845-a344-13f316f70651" />

- Repeat the previous step to create a new Organizational Unit
- Name this OU **_ADMINS**

- Both OUs should be in here now (screenshot below)

<img width="191" height="216" alt="Screenshot 2025-09-24 at 3 40 21 PM" src="https://github.com/user-attachments/assets/6ec112c9-fcb0-4454-b432-f6f7b4a9c4d8" />


- Let's add a new employee named Jane Doe.
- Right-click the **_ADMINS** folder -> New -> User

<img width="535" height="351" alt="Screenshot 2025-09-24 at 3 49 11 PM" src="https://github.com/user-attachments/assets/c05490cb-218a-486c-96e6-44afec4d094a" />

- Add her name and logon name (screenshot below)
- Set her password, click next
- Click Finish

<img width="433" height="374" alt="Screenshot 2025-09-24 at 3 49 48 PM" src="https://github.com/user-attachments/assets/aa5def99-f319-44ea-becb-da1aec0e231b" />

- Click on Jane Doe's account -> Properties

<img width="399" height="348" alt="Screenshot 2025-09-24 at 3 52 43 PM" src="https://github.com/user-attachments/assets/8904bb37-dbae-4991-b561-7bed218fff3f" />

- Click on the **Member Of** tab -> Add
- Type **domain admins** in the object names box (screenshot below)
- Click **Check Names**. Once you do this it the text box will underline what you type and it should show as Domain Admins (underlined).
- This means that the correct object name for Domain Admins was found.
- Click Apply -> OK

<img width="451" height="243" alt="Screenshot 2025-09-24 at 3 55 59 PM" src="https://github.com/user-attachments/assets/37475034-4a8a-4823-b1e4-d01ba3850641" />


- Now that Jane Doe has been added to the **Domain Admins** security group, let's log out of the Domain Controller
- We are going to login as Jane Doe.


## Step 3: Join client-1 to your Domain (mydomain.com)

- If you remember from the previous section of our lab we have already configured **client-1**'s DNS settings to point to **dc-1**'s **Private IP Address**
- If you haven't done so you can refer to the <a href="https://github.com/maggiemachuca/azure-prep-for-ad">previous section</a> and refer to Step 5. 

- Log into **client-1** using your original **local admin** credentials
- Click the Windows icon -> System -> **Rename this PC (advanced)**

- Click on the **Computer Name** tab -> Change (screenshot below)

<img width="408" height="464" alt="Screenshot 2025-09-24 at 4 11 12 PM" src="https://github.com/user-attachments/assets/c2bc1200-162c-4c50-b39e-14892f75c1d6" />

- Click **Domain** under **Member of**
- Type **mydomain.com**

<img width="317" height="385" alt="Screenshot 2025-09-24 at 4 12 44 PM" src="https://github.com/user-attachments/assets/9e6d105a-7c9c-48d1-9fc9-0dc5413c90ae" />

- You will get this window if you successfully pointed **client-1** to **dc-1**'s private IP address in the DNS settings in a step we did earlier. (screenshot below)
- This means **client-1** was able to find our domain controller.
- Type in the domain name along with the user credentials
- In this case it is **mydomain.com\jane_admin**
- Click OK

<img width="454" height="295" alt="Screenshot 2025-09-24 at 4 15 55 PM" src="https://github.com/user-attachments/assets/6b467bde-793a-499b-a122-430fec5070c5" />

- You should get this window if you were successful.
- You will then get a prompt to restart your computer.
- Click Restart Now

<img width="293" height="144" alt="Screenshot 2025-09-24 at 4 19 00 PM" src="https://github.com/user-attachments/assets/37697cfe-4133-4da6-9268-0825b652d51a" />

- Now we are going to log back into our domain controller **dc-1** to verify that **client-1** shows up in ADUC (Active Directory Users and Computers).
- Click the search bar
- Type in **Active Directory Users and Computers**

- Expand **mydomain.com**
- Click on the **Computers** folder
- You should be able to see **client-1** in our window. (screenshot below)
  
<img width="750" height="524" alt="Screenshot 2025-09-24 at 4 22 57 PM" src="https://github.com/user-attachments/assets/34474ead-a000-467e-aa41-077bc0ae7b05" />


- Next we are going to create a new **Organizational Unit** called **_CLIENTS**
- Right-click mydomain.com -> New -> Organizational Unit

<img width="662" height="402" alt="Screenshot 2025-09-24 at 4 25 29 PM" src="https://github.com/user-attachments/assets/545647f5-4b69-4f29-a106-4c12f2dcd1aa" />

- Now that you created this OU, let's put our client-1 machine into the **_CLIENTS** OU folder
- Click and drag client-1 into the **_CLIENTS** folder
- You'll get this pop-up, click Yes
  
<img width="466" height="169" alt="Screenshot 2025-09-24 at 4 27 51 PM" src="https://github.com/user-attachments/assets/6b83a785-e1fe-46c1-b457-e5dd8ef8f9ce" />














<br />
