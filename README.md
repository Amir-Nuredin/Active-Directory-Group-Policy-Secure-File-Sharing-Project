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

The main area of work for this week's project would be in Group Policy Management within Server Manager. To open it, I went to Server Manager, then on the top right selected tools, and the clicked Group Policy Management (IM3). Once I was inside, to find my domain, I expanded The forest, then domains, then found my domain "Corp.Local" under it (IM4). 

<img width="908" height="460" alt="image" src="https://github.com/user-attachments/assets/f3f3cb73-7f21-4c51-a611-40abc76769ed" />

IM3: Opening Group Policy Management in Server Manager

<img width="600" height="616" alt="image" src="https://github.com/user-attachments/assets/b304b29d-37e3-43f6-851b-ad5f62b6b8d8" />

IM4: Locating my Domain Within Group Policy Management

### Part 2: Creating Organizational Units (OUs)

This step was already done in a previous project, so nothing new needed to be done. I had created four different OUs: Users-OU, ITAdmins, Workstations, and Servers. 

### Part 3: Creating a Group Policy Object (GPO): Password Policy

This is where the real work begins. A Group Policy Object is essentially a rule or policy that is enforced for a user, group, or specific OU to manage and administer better security rules and practices. The first one I created was related to the Password Policy

1. **CREATING THE GPO.** To create this GPO, while in Group Policy Management, I right-clicked the domain, then selected the option "Create a GPO in this domain, and Link it here" so that later the GPO would apply to the entire domain (IM5). I then named it "Password Policy."
2. **EDITING THE GPO.** Now that I created the GPO, I needed to edit it to configure the different policies that I wanted to enforce. To navigate to the password policies section, first, I right-clicked the Password Policy GPO and selected edit. Then I navigated with this path: Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies and then → Password Policy (IM6).
3. **CONFIGURING POLICIES.** Now I was able to configure the different policies I wanted to enforce for passwords for all the users within my domain. There were a few options, but here are the four that I configured (IM7):
   - **Minimum password length → 12**: This meant that every user's password had to be at least 12 characters long
   - **Maximum Password Age → 30 days**: This meant that a user's password would expire after 30 days and would need to be changed
   - **Password Complexity → Enabled**: This meant that the Password created by the user had to meet Windows complexity requirements to be allowed to use (link to summary of what Windows complexity requirements are: https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).
   - **Enforce password history → 5**: This policy determines after how many password changes an old password can be used. I chose 5, so that means for example lets say a users password was ABC123. That user would have to have changed their password at least 5 times before using that previous password. This prevents password reuse and lazzines
4. **ENSURING GPO IS LINKED TO DOMAIN.** Once I configured the password policies, I applied them, and checked if they saved by going back to the main page of Group Policy Management, went under my domain, and clikced on the Password Policy GPO. I clicked Settings and confirmed that the policies were applied (IM8)

<img width="450" height="499" alt="image" src="https://github.com/user-attachments/assets/516da387-7fbb-4d25-9a45-e14a325ed59c" />

IM5: Creating GPO in Group Policy Management

<img width="793" height="301" alt="image" src="https://github.com/user-attachments/assets/9d851449-3561-483f-b6c7-b2b9da11072c" />

IM6: Navigating to Password Policies within my GPO for Passwords

<img width="781" height="269" alt="image" src="https://github.com/user-attachments/assets/c78f3fc8-c50b-4c7c-b12f-aea84abb2c2f" />

IM7: Configuring Password Policies Within GPO

<img width="700" height="450" alt="Screenshot 2026-05-25 102422" src="https://github.com/user-attachments/assets/c6991df3-3b7f-4d2e-b198-0182ca79c9d4" />

IM8: Confirming GPO Rules Applied to Domain

### Part 4: Configuring Account Lockout Policy

Another Policy I wanted to configure within the "Password Policies" GPO was Account Lockout policy. This policy essentially configures after how many incorrect login attempts will that account be locked out of being logged in for and for how long. To do this, I went back through the same path to reach the password policies, and right under it was account lockout policy. Once I was there, I configured the three settings that were available. The first option was account lockout duration, and I chose fifteen minutes. The next option was the lockout threshold, so how many attempts would it take to lock that account, and I chose 5. A third option was for how long should that lockout occur before the amount reset, and I also chose 15 minutes for that as well (IM9). These polices help stop brute-force attacks as if a threat actor was trying to force their way into logging into an account, they could only have so many attempts before they would get locked out, giving more time for incident response to get on top of things. 

<img width="786" height="282" alt="Screenshot 2026-05-25 102751" src="https://github.com/user-attachments/assets/7ef29274-4189-4345-b45d-aad25c587b43" />

IM9: Account lockout Policy Configuration Within Password Policy GPO








