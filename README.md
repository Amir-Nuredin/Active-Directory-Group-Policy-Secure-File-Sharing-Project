# Active Directory Group Policy and Secure File Sharing Project

## Objective

This lab/project aims to develop practical skills in administering and securing a Windows-based enterprise environment through the use of Group Policy and file access control. In this lab, I focused on moving beyond basic domain setup to actively managing and enforcing organizational policies within an Active Directory environment. Using Windows Server 2022, I created and configured Group Policy Objects (GPOs) to implement password policies, account lockout settings, and user-based restrictions, such as limiting access to system tools like the Control Panel and Command Prompt. I also organized domain resources using Organizational Units (OUs) to apply targeted policies and demonstrate the principle of least privilege. Additionally, I configured a secure file sharing system by creating a shared directory and applying both share-level and NTFS permissions to control user access. This allowed me to simulate real-world access control scenarios, where standard users are restricted to read-only access while administrative users retain full control. The lab concluded with testing and validation from a domain-joined Windows 10 machine, ensuring that all policies, restrictions, and permissions were correctly enforced. This project demonstrates the implementation of centralized management, access control, and security best practices within an enterprise-style Windows environment.

### Skills Learned

This project has provided hands-on experience with the following security solutions/skills:

- Group Policy Object (GPO) creation, configuration, and domain-level enforcement
- Implementation of password policies (complexity, expiration, history) in Active Directory
- Application of user-based restrictions using Group Policy (Control Panel and Command Prompt access control)
- Understanding and application of the principle of least privilege in enterprise environments
- Configuration of shared folders and network file access in Windows Server
- Troubleshooting GPO application, policy conflicts, and network file access issues

### Tools Used
- Windows Server 2022 (Desktop Experience)
- Group Policy Management Console (GPMC)
- Active Directory Users and Computers (ADUC)
- Windows 10 VM (Domain-Joined Client VM)
- File Explorer (Shared Folder Configuration & Access)
- VirtualBox (Virtualization Platform)
- Server Manager (Windows Server Administration)
- Command-Line Tools (gpupdate, gpresult, ipconfig, ping, hostname)

## Current Lab Environment Architecture/Setup

Below are screenshots of my lab Architecture before and after this week's lab/project (IM1&2). There were no new hardware additions to the environment, but Group Policies were added, as well as new NTFS file permissions

<img width="800" height="700" alt="image" src="https://github.com/user-attachments/assets/cdc9ad74-4349-4972-a7cd-9e7179dbc8f5" />
<img width="800" height="800" alt="image" src="https://github.com/user-attachments/assets/aa5fe393-1dcd-4fd4-a1d6-f7c86f7aef0a" />

IM1&2: Current Lab Environment/Architecture Before and After this Week's Lab/Project

## Steps/Procedure

### Part 1: Opening Group Policy Management

The main area of work for this week's project would be in Group Policy Management within Server Manager. To open it, I went to Server Manager, then on the top right selected tools, and then clicked Group Policy Management (IM3). Once I was inside, to find my domain, I expanded the forest, then domains, then found my domain "Corp.Local" under it (IM4). 

<img width="908" height="460" alt="image" src="https://github.com/user-attachments/assets/f3f3cb73-7f21-4c51-a611-40abc76769ed" />

IM3: Opening Group Policy Management in Server Manager

<img width="600" height="616" alt="image" src="https://github.com/user-attachments/assets/b304b29d-37e3-43f6-851b-ad5f62b6b8d8" />

IM4: Locating my Domain Within Group Policy Management

### Part 2: Creating Organizational Units (OUs)

This step was already done in a previous project, so nothing new needed to be done. I had created four different OUs: Users-OU, ITAdmins, Workstations, and Servers. 

### Part 3: Creating a Group Policy Object (GPO): Password Policy

This is where the real work begins. A Group Policy Object is essentially a rule or policy that is enforced for a user, group, or specific OU to manage and administer better security rules and practices. The first one I created was related to the Password Policy

1. **CREATING THE GPO.** To create this GPO, while in Group Policy Management, I right-clicked the domain, then selected the option "Create a GPO in this domain, and Link it here" so that later the GPO would apply to the entire domain (IM5). I then named it "Password Policy."
2. **EDITING THE GPO.** Now that I had created the GPO, I needed to edit it to configure the policies I wanted to enforce. To navigate to the password policies section, first, I right-clicked the Password Policy GPO and selected edit. Then I navigated with this path: Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies and then → Password Policy (IM6).
3. **CONFIGURING POLICIES.** Now I was able to configure the different policies I wanted to enforce for passwords for all the users within my domain. There were a few options, but here are the four that I configured (IM7):
   - **Minimum password length → 12**: This meant that every user's password had to be at least 12 characters long
   - **Maximum Password Age → 30 days**: This meant that a user's password would expire after 30 days and would need to be changed
   - **Password Complexity → Enabled**: This meant that the Password created by the user had to meet Windows complexity requirements to be allowed to use (link to summary of what Windows complexity requirements are: https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).
   - **Enforce password history → 5**: This policy determines how many password changes an old password can be used after. I chose 5, so that means, for example, let's say a user's password was ABC123. That user would have had to have changed their password at least 5 times before using that previous password. This prevents password reuse and laziness
4. **ENSURING GPO IS LINKED TO DOMAIN.** Once I configured the password policies, I applied them, and checked if they saved by going back to the main page of Group Policy Management, going under my domain, and clicking on the Password Policy GPO. I clicked Settings and confirmed that the policies were applied (IM8)

<img width="450" height="499" alt="image" src="https://github.com/user-attachments/assets/516da387-7fbb-4d25-9a45-e14a325ed59c" />

IM5: Creating a GPO in Group Policy Management

<img width="793" height="301" alt="image" src="https://github.com/user-attachments/assets/9d851449-3561-483f-b6c7-b2b9da11072c" />

IM6: Navigating to Password Policies within my GPO for Passwords

<img width="781" height="269" alt="image" src="https://github.com/user-attachments/assets/c78f3fc8-c50b-4c7c-b12f-aea84abb2c2f" />

IM7: Configuring Password Policies Within GPO

<img width="700" height="450" alt="Screenshot 2026-05-25 102422" src="https://github.com/user-attachments/assets/c6991df3-3b7f-4d2e-b198-0182ca79c9d4" />

IM8: Confirming GPO Rules Applied to Domain

### Part 4: Configuring Account Lockout Policy

Another Policy I wanted to configure within the "Password Policies" GPO was the Account Lockout policy. This policy essentially configures after how many incorrect login attempts will that account be locked out of being logged in for and for how long. To do this, I went back through the same path to reach the password policies, and right under it was the account lockout policy. Once I was there, I configured the three available settings. The first option was account lockout duration, and I chose fifteen minutes. The next option was the lockout threshold, so how many attempts would it take to lock that account, and I chose 5. A third option was for how long the lockout occurs before the amount resets, and I also chose 15 minutes for that as well (IM9). These policies help stop brute-force attacks, as if a threat actor were trying to force their way into an account; they could only make so many attempts before they would get locked out, giving more time for incident response to get on top of things. 

<img width="786" height="282" alt="Screenshot 2026-05-25 102751" src="https://github.com/user-attachments/assets/7ef29274-4189-4345-b45d-aad25c587b43" />

IM9: Account Lockout Policy Configuration Within Password Policy GPO

### Part 5: Forcing Policy Update. 

To ensure that both these policies were applied, I needed to run the command "gpupdate /force" on the Windows 10 VM so that the policies would be applied to that machine (IM10). I could also do it on the Domain Controller as well, but it wasn't needed. 

<img width="402" height="138" alt="Screenshot 2026-05-25 103248" src="https://github.com/user-attachments/assets/d3188128-e0b6-4568-9999-d049a2213cc7" />

IM10: Forcing Policy Update on Windows 10 Machine

### Part 6: Testing Password and Lockout Policies

I now wanted to test and see if both of these types of policies were working within my domain as intended. 

1. **TESTING PASSWORD POLICIES.** First, I wanted to test the password policies. I went onto the Windows 10 VM and logged in as a standard user account "amir.user." I did Ctrl+Alt+Delete (for VirtualBox, it is left Ctrl+Delete) and chose "Change a Password." I first tried to change the password to something I knew wouldn't work so that I could see if the policies were working. I changed the password to "ABCD1234," and it didn't work, giving me a message that the password didn't meet the Complexity Requirements (IM11). I then tried a more secure and longer password that met the Complexity Requirements, and it worked, confirming that the password policies were working as intended
2. **TESTING ACCOUNT LOCKOUT POLICY.** To test this policy, I simply tried to log in to the standard user account with the wrong password on purpose five times. After the sixth time of trying to log in, I got a message saying that the account had been locked out, confirming that the lockout policy was working as intended as well (IM12). Since I still needed the account to use for other things and I couldn't wait the fifteen minutes, I went on my DC and manually unlocked the account in Active Directory (normally in a real company setting, only the administrator could do this and would need proof that the real user was having issues logging in and not a threat actor) (IM13).

<img width="556" height="239" alt="Screenshot 2026-05-25 103647" src="https://github.com/user-attachments/assets/88e8dfce-45b9-440a-8065-cf10345f45ac" />

IM11: Confirming Non-Complex Password Would be Blocked by Password Policy

<img width="475" height="475" alt="Screenshot 2026-05-25 105718" src="https://github.com/user-attachments/assets/a184e1d5-65b3-4d6d-8122-18c14b3e1b9a" />

IM12: Account Locked Out Due to Multiple Incorrect Login Attempts

<img width="475" height="500" alt="image" src="https://github.com/user-attachments/assets/99c4371f-45ff-4542-9900-54778e9732ea" />

IM13: Optional Manual Unlock of User Account

### Part 7: Creating Desktop Restriction GPO

The second kind of GPO I wanted to create would only apply to standard users. This GPO of "Desktop Restrictions" would mean that standard users wouldn't be able to access specific applications that were blocked by policies that an administrator would create. This is great in a real environment, as typically you wouldn't want standard users to be able to access any application that they want. Here is how I did that. 

1. **CREATING NEW GPO.** Since this GPO wasn't going to be applied to the whole domain, I didn't want to create it right under Corp.Local. Instead, I right-clicked under Group Policy Objects, selected New, and named the GPO "User Restrictions (IM14)."
2. **EDITING THE GPO.** To configure the restrictions that I wanted, I right-clicked the GPO and selected "Edit." I then navigated through User Configuration → Policies and → Administrative Templates.
3. **CONFIGURING USER RESTRICTIONS.** Now I was able to restrict any programs/applications that I wanted. I first wanted to disable the control panel for any standard users. This was under Administrative Templates (IM15). To disable the control panel, I clicked the Control Panel folder and found the option "Prohibit access to Control Panel." I double-clicked that and enabled that option, then applied and saved it (IM16). Disabling the control panel can be useful, as you wouldn't want normal users to have the ability to start or disable any important running programs; this should only be up to the admins and higher-privileged users. I next wanted to disable Command Prompt for standard users. To do this, I went back to Administrative Templates, then clicked the System Folder. I then looked for the option "Prevent access to the command prompt (IM17)." I double-clicked that option and enabled it, then applied the configuration and saved it (IM18). Disabling the Command Prompt is also useful as users might accidentally run commands that they aren't supposed to, which could negatively impact a network.

<img width="749" height="368" alt="Screenshot 2026-05-25 112553" src="https://github.com/user-attachments/assets/ffc8f2d4-0930-444f-afcf-69b1eeefe534" />

IM14: Creating User Restrictions GPO

<img width="786" height="411" alt="image" src="https://github.com/user-attachments/assets/9afa4c80-ac0e-4fff-a459-c0b2824377e1" />

IM15: Navigating to Control Panel Settings

<img width="775" height="620" alt="image" src="https://github.com/user-attachments/assets/16236856-e8c8-4080-99bb-b001870f0dd1" />

IM16: Disabling Control Panel Within GPO

<img width="786" height="417" alt="image" src="https://github.com/user-attachments/assets/12cb1d4b-11a8-4b5b-80eb-538621daa410" />

IM17: Navigating to Command Prompt Settings

<img width="600" height="550" alt="image" src="https://github.com/user-attachments/assets/13f2ba59-d168-4ceb-9002-1f558a79b8da" />

IM18: Disabling Command Prompt Within GPO

### Part 8: Linking User Restrictions GPO to Users-OU OU

Now that I had created the User Restrictions GPO and configured the policies that I wanted, I needed to link the GPO to the correct OU so that it would apply only where I needed to and not anywhere else. This is important because if I linked this GPO to the OU, for example, the admin accounts, I would restrict their access to resources that they need to perform their role. To do this, I went to the OU that I wanted to link it to, which was "Users-OU." I right-clicked it and selected "Link an Existing GPO (IM19)." After that, the prompt asked me which GPO I wanted to link, and I chose User Restrictions (IM20). Now, after completing this, the GPO was under the Users-OU OU (IM21).  

<img width="426" height="577" alt="image" src="https://github.com/user-attachments/assets/8a75a6b4-0a00-4dcf-9d02-0f9911559a17" />
<img width="700" height="475" alt="Screenshot 2026-05-25 113141" src="https://github.com/user-attachments/assets/e995ab84-00ed-422d-a812-438cacc81663" />

IM19&20: Linking User Restriction GPO to User-OU OU

<img width="598" height="366" alt="image" src="https://github.com/user-attachments/assets/c9e6ab5e-be18-4062-a02d-2f52669d1870" />

IM21: User Restrictions GPO under Users-OU OU

### Part 9: Testing User Restrictions

Now I am going to test those restriction policies that I created to see if they only applied to standard users and nobody else

1. **APPLYING POLICY.** To make sure the policies applied, I ran the "gpupdate /force" command on the Windows 10 VM to apply the policies to the machine.
2. **TESTING USER RESTRICTIONS ON STANDARD AND ADMIN ACCOUNTS.** I first tested these restrictions on the standard user account. I logged back in and ran Windows + R to use the Run feature and typed in "Control" to run Control Panel. When I hit enter, I got an error saying that certain restrictions were blocking this action, ensuring that the Control Panel policy was working (IM22). I then tried to open the Command Prompt with the standard user account, and when it opened, it said that CMD was disabled by the administrator, confirming that the Command Prompt Policy was working as well (IM23). Lastly, I wanted to ensure that these policies were only applying to standard users and nobody else. I logged into one of the admin accounts and tried opening up both the Control Panel and Command Prompt using the same process, and they both worked, ensuring that the GPO was only applying to standard users (IM24).

<img width="561" height="130" alt="Screenshot 2026-05-25 113618" src="https://github.com/user-attachments/assets/3d1c6b4f-cbc0-45f3-b4ff-b4ced62aba75" />

IM22: Control Panel Disabled on Standard User Account

<img width="509" height="135" alt="Screenshot 2026-05-25 113646" src="https://github.com/user-attachments/assets/e3ad5ea7-745e-4f69-9629-b503e1869791" />

IM23: Command Prompt Disabled on Standard User Account

<img width="700" height="600" alt="Screenshot 2026-05-25 114239" src="https://github.com/user-attachments/assets/343f3f2d-6c68-4be7-963d-47188ce2d5b2" />

IM24: Both Command Prompt and Control Panel are working on the Admin Account

### Part 10: Company File Permission Simulation and Sharing Setup

The Last Part of this project was simulating giving different permissions to a company file within the domain so that different users had different levels of access to those company documents/folders. 

1. **CREATING COMPANY FOLDER.** For this step, I simply went to my Windows Server, went to the C: drive, and created a new folder named "CompanyFiles (IM25)." This is the main folder that I would use to grant different permissions.
2. **SHARING THE FOLDER TO THE DOMAIN.** Now to share this folder, I right-clicked it, then navigated to Properties, then the Sharing tab, then Advanced sharing, then clicked the checkbox that said: "Share this folder (IM26)."
3. **CONFIGURING SHARE PERMISSIONS.** These permissions are only for who can access the shared folder over the network. In the same tab, I selected permissions, and then added the two groups I wanted to configure permissions for, Employees and ITAdmins. For Employees, they only get Read access (IM27), and IT admins get full control over the folder (IM28). I then applied and saved the changes.

<img width="571" height="211" alt="image" src="https://github.com/user-attachments/assets/8ef0a0b6-e85d-473d-b976-202228b31acf" />

IM25: Creating Shared Folder "CompanyFiles."

<img width="352" height="355" alt="Screenshot 2026-05-25 123207" src="https://github.com/user-attachments/assets/1d93c639-8dc3-49b7-967b-f33c180b77bd" />

IM26: Sharing Folder to Domain

<img width="360" height="442" alt="image" src="https://github.com/user-attachments/assets/1ccb2fca-4c51-44b9-945d-7371c91a0b9e" />
<img width="358" height="443" alt="image" src="https://github.com/user-attachments/assets/16c49f26-c897-4d60-bc11-696037ed04d4" />

IM27&28: Configuring Share Permissions for Employees and ITAdmins Groups. 

### Part 11: Configuring NTFS Permissions

This step is an add-on to the last one, as these are the real permissions for the file when it comes to who can edit, modify, or delete files in the folder. This is an important concept known as least privilege. It is the idea that users and administrators should only have access to what they need, and nothing more. This is important because it can help seperate permissions for different users. For example, if there is a high level document that only executives can read, admins can read and modify, and normal users can't see at all, we can configure the permissions to apply to that scnerio. To do this, I right-clicked the file again, select properties, and then security. I added the two groups again, Employees and ITAdmins, and configured their permissions (IM29). For the Employees group, they got Read permissions (as well as read and execute and list folder contents, which go under Read) (IM30), and the ITAdmins group got full control over the file (IM31).  

<img width="755" height="465" alt="Screenshot 2026-05-25 115750" src="https://github.com/user-attachments/assets/4d50f7fe-396d-44e6-a29b-5b85ad16dae7" />

IM29: Adding Both Groups under the Security tab

<img width="326" height="285" alt="Screenshot 2026-05-25 115852" src="https://github.com/user-attachments/assets/ba4c9f45-6aa9-42eb-9106-e97fc283f81a" />
<img width="359" height="301" alt="Screenshot 2026-05-25 115907" src="https://github.com/user-attachments/assets/1643f144-b43e-4ba0-81d9-83d0ff179f4f" />

IM30&31: Configuring Access Permissions for Both Groups

































































































