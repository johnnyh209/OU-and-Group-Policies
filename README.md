# OU-and-Group-Policies

This is a project where I create Organizational Units and Group Policies. The technologies used are VirtualBox, Windows Server 2019, and Windows 10 Enterprise. 

**Background:** I currently have a Windows Server 2019 system setup to manage the Active Directory, and 3 Windows 10 Enterprise systems (1 system is for help desk, 2 systems are for regular employees).

![1  Showing list of devices in the domain](https://github.com/johnnyh209/OU-and-Group-Policies/assets/33064730/4d039350-7c6f-4bcc-98fe-3d27b4f2f60a)

# Creating Organizational Units

**Step 1:** My first step was to create Organizational Units (OUs) so that I can manage the different user types easily. In `Server Manager` on my Windows Server 2019 system, I opened up `Active Directory Users and Computers`. I then right-clicked the domain I have created, `laborg.local`, clicked on `New`, and then clicked on `Organizational Unit`. A new window opens up, prompting me to enter a name for the OU. For this OU, I will be naming it `Employee`.

![2  Adding OU Part 1](https://github.com/johnnyh209/OU-and-Group-Policies/assets/33064730/f9d864f8-5834-46c6-96e9-ebc0cc7f1191)
![3  Naming the OU](https://github.com/johnnyh209/OU-and-Group-Policies/assets/33064730/b7eb23f2-6487-4534-96f7-7bdddf362e50)

**Step 2:** With the `Employee` OU created, I can then move users into this OU. In my `Users` folder, I located the `Staff LabOrg` and `Staff2 LabOrg` users, and dragged them into the `Employee` OU folder. 

![6  Moving Users to OU](https://github.com/johnnyh209/OU-and-Group-Policies/assets/33064730/b368d61d-f401-45e7-8f92-9f4c2c66510f)

Having done that, the inside of the `Employee` OU looked like the following for me:

![7  Showing Employee OU](https://github.com/johnnyh209/OU-and-Group-Policies/assets/33064730/98b5828e-99a9-4bd6-891f-7b49ccc57cbb)

**Step 3:** I then created my second OU, `IT`. I performed the same steps that I did when creating the `Employee` OU. 

![8  IT OU](https://github.com/johnnyh209/OU-and-Group-Policies/assets/33064730/25ab67cc-abcd-44e4-9087-a29594d9604b)

# Creating and Applying Group Policies

**Step 1:** I used the Group Policy Management tool in Server Manager to create and manage group policies. To open the Group Policy Management tool, navigate through the following: `Tools > Group Policy Management`. 
For this exercise, I will be creating a group policy specific to the `Employee` OU. I first right-clicked on the `Employee` OU, and clicked on `Create a GPO in this domain, and Link it here...`.

![10  Create GPO Part 1](https://github.com/johnnyh209/OU-and-Group-Policies/assets/33064730/7a8f37a5-773c-403b-8fac-9da7efbc37d1)

This opened up a new window where I entered a name for the GPO. For this particular GPO, I named it `Password Age`. As the name suggests, I wanted to assign a group policy to the `Employee` OU that defined how long a password can be used before requiring users in this OU to change their passwords. 

**Step 2:** With the GPO created, the next step is to edit (or define) what the policy will consist of. I right-clicked on the newly created GPO, and clicked on `Edit`. 

![12  Edit GPO](https://github.com/johnnyh209/OU-and-Group-Policies/assets/33064730/725eedea-92e0-4b19-8935-b5a4b05bf2ee)

Doing so opened up the `Group Policy Management Editor`. In this editor, I can define a group policy for Computer Configuration or User Configuration. For this attempt, I wanted to apply the password policy to Computer Configuration. I navigated through the following: `Computer Configuraiton > Policies > Windows Settings > Security Settings`. Within `Security Settings`, I double-clicked on `Account Policies`.

![13  Edit GPO Part 2](https://github.com/johnnyh209/OU-and-Group-Policies/assets/33064730/a257b5fe-b612-4815-9313-a383074e9143)

I then double-clicked on `Password Policy` and then double-clicked on `Maximum password age` open up `Maximum password age Properties`. This allows me to define in days the max length of time in which a password can be used for. For this attempt, I decided to enter in 90 days. 

![16  Edit GPO Part 5](https://github.com/johnnyh209/OU-and-Group-Policies/assets/33064730/e824978a-74ce-4826-a02c-17abb35dea02)
![15  Edit GPO Part 4](https://github.com/johnnyh209/OU-and-Group-Policies/assets/33064730/7f8f876d-ef4a-4d6e-803d-4c0d46d573d9)

**Step 3:** With the password policy set and enabled, the policy should affect both `Staff LabOrg` and `Staff2 LabOrg` users inside of the `Employee` OU. To check this, I opened up the virtual machine for `Staff LabOrg` and logged in. Theoretically, logging in should have updated the group policy on this system. As practice, and to further ensure that the group policy has been updated on this user and system, I opened up `CMD` with administrative rights. I then ran the command `gpupdate /force` to update the group policy. 

![18  Running gpupdate](https://github.com/johnnyh209/OU-and-Group-Policies/assets/33064730/6fae98af-8279-41d5-ad43-ab2c79dce57a)

After running `gpupdate /force`, I then ran `gpresult /r` to check the current group policy configuration. However, the group policy `Password Age` was not listed. The only policy listed was the `Default Domain Policy` that applies to all of the users/OUs within the `laborg.local` domain. 

![pUQQRnncHB](https://github.com/johnnyh209/OU-and-Group-Policies/assets/33064730/59018a7b-32d2-4ca2-a4a1-5c0117458c0d)

What I came to find out was that it usually is not a good idea to create password-related group policies specific to OUs. In a post on [spiceworks](https://community.spiceworks.com/topic/2471197-applying-group-policy), a user says that "Password settings are normally applied in the default domain GPO, and unless you start using Fine Grained Password Policies, you cannot have multiple different password policies per OU". With that in mind, I started from square one again; this time I decided to create a group policy that forces screensaver after a system has been idle for a certain amount of time.

