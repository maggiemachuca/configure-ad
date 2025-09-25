<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
In this tutorial we build an Active Directory environment in Azure, complete with domain controller setup, client domain joining, remote access configuration, and automated user provisioning with PowerShell.
<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell ISE

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (22H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: Install Active Directory Domain Services on DC-1
- Step 2: Create a Domain Admin user within the domain
- Step 3: Join CLIENT-1 to your Domain (mydomain.com)
- Step 4: Setup Remote Desktop for Non-Admin Users on CLIENT-1
- Step 5: Create Domain Users & Test Login as a User

<h2>Deployment and Configuration Steps</h2>

## Step 1: Install Active Directory Domain Services on DC-1

- Navigate to **Server Manager**
- Click on **Add roles and features**
- Once you click this we will be going through the installation wizard
  
<img width="623" height="335" alt="Screenshot 2025-09-24 at 3 00 37 PM" src="https://github.com/user-attachments/assets/16339968-7ecc-4b39-8b2a-ba686f61b60c" />

- On Server Selection, choose the domain controller.
- In this case it is **DC-1**

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
- Let's set up the **DC-1** VM in a new Active Directory forest.

- On **Server Manager**, click the flag icon and then click **Promote this server to a domain controller** (screenshot below)
  
<img width="390" height="194" alt="Screenshot 2025-09-24 at 3 13 35 PM" src="https://github.com/user-attachments/assets/695d73b6-3c6c-4ea5-98f1-27db17d871dd" />

- Click **Add a new forest**
- In the root domain name, type: **mydomain.com**
- Click Next.
  
<img width="755" height="309" alt="Screenshot 2025-09-24 at 3 15 23 PM" src="https://github.com/user-attachments/assets/b27c8044-edd3-47de-9a44-95fdad6fc4cd" />

- Set your password in the next section
- Click Next.

<img width="465" height="98" alt="Screenshot 2025-09-24 at 3 17 20 PM" src="https://github.com/user-attachments/assets/309f4a7e-0dae-4173-9df7-75ad425f465f" />

- In DNS options, uncheck **Create DNS delegation**
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


## Step 3: Join CLIENT-1 to your Domain (mydomain.com)

- If you remember from the previous section of our lab we have already configured **CLIENT-1**'s DNS settings to point to **DC-1**'s **Private IP Address**
- If you haven't done so you can refer to the <a href="https://github.com/maggiemachuca/azure-prep-for-ad">previous section</a> and refer to Step 5. 

- Log into **CLIENT-1** using your original **local admin** credentials
- Click the Windows icon -> System -> **Rename this PC (advanced)**

- Click on the **Computer Name** tab -> Change (screenshot below)

<img width="408" height="464" alt="Screenshot 2025-09-24 at 4 11 12 PM" src="https://github.com/user-attachments/assets/c2bc1200-162c-4c50-b39e-14892f75c1d6" />

- Click **Domain** under **Member of**
- Type **mydomain.com**

<img width="317" height="385" alt="Screenshot 2025-09-24 at 4 12 44 PM" src="https://github.com/user-attachments/assets/9e6d105a-7c9c-48d1-9fc9-0dc5413c90ae" />

- You will get this window if you successfully pointed **CLIENT-1** to **DC-1**'s private IP address in the DNS settings in a step we did earlier. (screenshot below)
- This means **CLIENT-1** was able to find our domain controller.
- Type in the domain name along with the user credentials
- In this case it is **mydomain.com\jane_admin**
- Click OK

<img width="454" height="295" alt="Screenshot 2025-09-24 at 4 15 55 PM" src="https://github.com/user-attachments/assets/6b467bde-793a-499b-a122-430fec5070c5" />

- You should get this window if you were successful.
- You will then get a prompt to restart your computer.
- Click Restart Now

<img width="293" height="144" alt="Screenshot 2025-09-24 at 4 19 00 PM" src="https://github.com/user-attachments/assets/37697cfe-4133-4da6-9268-0825b652d51a" />

- Now we are going to log back into our domain controller **DC-1** to verify that **CLIENT-1** shows up in ADUC (Active Directory Users and Computers).
- Click the search bar
- Type in **Active Directory Users and Computers**

- Expand **mydomain.com**
- Click on the **Computers** folder
- You should be able to see **CLIENT-1** in our window. (screenshot below)
  
<img width="750" height="524" alt="Screenshot 2025-09-24 at 4 22 57 PM" src="https://github.com/user-attachments/assets/34474ead-a000-467e-aa41-077bc0ae7b05" />


- Next we are going to create a new **Organizational Unit** called **_CLIENTS**
- Right-click mydomain.com -> New -> Organizational Unit

<img width="662" height="402" alt="Screenshot 2025-09-24 at 4 25 29 PM" src="https://github.com/user-attachments/assets/545647f5-4b69-4f29-a106-4c12f2dcd1aa" />

- Now that you created this OU, let's put our CLIENT-1 machine into the **_CLIENTS** OU folder
- Click and drag CLIENT-1 into the **_CLIENTS** folder
- You'll get this pop-up, click Yes
  
<img width="466" height="169" alt="Screenshot 2025-09-24 at 4 27 51 PM" src="https://github.com/user-attachments/assets/6b83a785-e1fe-46c1-b457-e5dd8ef8f9ce" />


## Step 4: Setup Remote Desktop for Non-Admin Users on CLIENT-1

- Log into CLIENT-1 as the user **jane_admin**
- Right click on the Start menu -> System -> Remote Desktop 
- Click on **Select users that can remotely access this PC** (screenshot below)
- Click add

<img width="513" height="698" alt="Screenshot 2025-09-24 at 4 36 15 PM" src="https://github.com/user-attachments/assets/f66f4239-7e33-46d0-ab4f-56b2a628bedd" />

- Type in **Domain Users** to allow non-admin users to logon remotely
- Click Check Names
- Click OK

<img width="447" height="245" alt="Screenshot 2025-09-24 at 4 38 20 PM" src="https://github.com/user-attachments/assets/0f924d2a-74fe-4105-ad0d-c82781c66bda" />

- Now all domain users should be able to log onto this computer.
- This could be done using Group Policy as well 

<img width="373" height="331" alt="Screenshot 2025-09-24 at 4 39 53 PM" src="https://github.com/user-attachments/assets/c0d0c59f-45e8-4fa3-8923-09948d13e347" />


## Step 5: Create Domain Users & Test Login as a User

- Log onto DC-1 as the user **jane_admin**
- Click on Start Menu
- In the search bar type in **Windows PowerShell ISE**, right-click and **Run as administrator**

- Once it opens, click on **New script** on the top-left corner
- Name the file **Create users** and save it onto the desktop
- We are going to use a script from github to create our users
- <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">Click here</a> to get the script (it will open a new page on your browser)
- On github you should see an on option to **Copy raw file**. Click this.

<img width="1108" height="156" alt="Screenshot 2025-09-24 at 5 02 39 PM" src="https://github.com/user-attachments/assets/e12bd1cd-b642-40d7-9d9b-11a14cb8f3d5" />

- Now paste what you just copied into **Windows PowerShell ISE**
- If it successfully pasted your window should look something like this: (screenshot below)
- Save one more time

<img width="1277" height="744" alt="Screenshot 2025-09-24 at 5 07 38 PM" src="https://github.com/user-attachments/assets/797e1cfa-79a3-47ab-9b6f-fa1cecdf9c18" />

- Click **Run script**
- You'll notice that the script is running now and hundreds of users are being created (screenshot below)
- As you wait for this script to finish, observe the contents of the script
- Take note that there is a **default password** set for all of these users. We will use this in a few steps.
  
<img width="248" height="283" alt="Screenshot 2025-09-24 at 5 08 33 PM" src="https://github.com/user-attachments/assets/5e85e484-e252-4e4a-ae48-8bc9570d220b" />

- This script will place these users into the appropriate OU, which is **_EMPLOYEES**
- Since we are creating so many users this may take a few minutes to finish.
- **You do not have to wait for the script to finish to continue.**

- Click the Start Menu and type **Active Directory Users and Computers**
- Expand mydomain.com
- Click on _EMPLOYEES
- Note all of the users in this Window that the PowerShell script created.

<img width="747" height="519" alt="Screenshot 2025-09-24 at 5 12 51 PM" src="https://github.com/user-attachments/assets/e68d3fab-2bcd-4bb4-9763-f0be28c2c696" />

- You may start this step if your script is finished or not.
- Pick a user from your list of users (note: We will have different users from each other, so please take note of the one you select)
- In my example I'll pick **duv.ros** as the user.
- Log out of your **CLIENT-1** machine if you are still logged in.
  
<img width="150" height="28" alt="Screenshot 2025-09-24 at 5 19 37 PM" src="https://github.com/user-attachments/assets/58b76dc1-b87f-40af-96d0-1e594247a16b" />

- Log into **CLIENT-1** as the user you picked
- Don't forget to type in the domain name when logging back in.
  
<img width="434" height="234" alt="Screenshot 2025-09-24 at 5 21 31 PM" src="https://github.com/user-attachments/assets/63a9ebc9-ce46-4307-8a82-17e22bb6be05" />

- You should be logged into **CLIENT-1** now as the user (duv.ros)
- Open the Command Prompt. Note how your username appears here (screenshot below)

<img width="424" height="111" alt="Screenshot 2025-09-24 at 5 23 42 PM" src="https://github.com/user-attachments/assets/4897d1c7-6908-4c84-9c6e-af534ec5f725" />

- That's it for now. You can log out of this **CLIENT-1** VM for now.

- In the next section we will be applying Group Policy and Managing Accounts! (to be continued)

# Summary at a High-Level Overview:

- **Deployed Active Directory in Azure**: Installed and configured Active Directory Domain Services on a Windows Server 2022 VM (DC-1) hosted in Microsoft Azure, promoting it to a domain controller within a new forest.

- **Domain Admin Setup**: Created custom Organizational Units (_EMPLOYEES, _ADMINS), added a new domain admin account (jane_admin), and assigned appropriate group memberships using Active Directory Users and Computers.

- **Client Domain Join**: Configured DNS and successfully joined a Windows 10 client VM (CLIENT-1) to the domain, verifying connectivity and object creation (**Domain Admins** and **Domain Users**) within ADUC.

- **Enabled RDP for Domain Users**: Configured Remote Desktop access on the client machine to allow non-admin users remote access by assigning the “Domain Users” group.

- **Automated User Provisioning**: Used a PowerShell script to create hundreds of domain users automatically and tested domain login as one of the new users on the joined client VM.


<br />
