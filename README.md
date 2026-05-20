# User Account Management

## Objective:
Demonstrating enterprise-level Active Directory user lifecycle management including configuring organizational units, user creation, group management, Group Policy configuration, and account offboarding using the Active Directory GUI tools on Windows Server 2022.

## Environment:
- Hypervisor: VMware Workstation Pro
- Server OS: Windows Server 2022 Standard Evaluation
- Domain Controller: DC01 (corp.local)
- Tools Used: Active Directory Users and Computers, Group Policy Management Console

## Skills Demonstrated:
- Organizational Unit (OU) design
- User account creation
- Security group management
- Group Policy Object (GPO) configuration
- Account offboarding procedure

## Step 1 — Created Organizational Unit Structure
Opened Active Directory Users and Computers (ADUC) and created an organizational unit.

<br>

<img width="1226" height="874" alt="UAM2" src="https://github.com/user-attachments/assets/7a102da4-06dc-4059-9617-5f9e0f7a2d9f" />

<br>
<br>

Created 5 OUs: Finance, HR, IT, Sales, and Disabled_Accounts to mirror a real company department structure. This will help manage to keep users organized, and allows implementation of Group Policies to be applied at the department level rather than domain-wide.

<br>

<img width="1223" height="876" alt="UAM3" src="https://github.com/user-attachments/assets/0f78be96-fbf0-4735-8e63-1da09398f3f4" />

## Step 2 — Manual User Creation
Created individual user accounts inside their respective department OUs using Active Directory Users and Computers. Each account was configured with a logon name following the firstname.lastname naming convention, an initial password, and the option to force a password change at first logon — standard practice in enterprise environments.
<br>

<img width="1226" height="877" alt="UAM4" src="https://github.com/user-attachments/assets/a8f44af3-b90a-4013-ad2f-9723cd84faa2" />
<img width="1224" height="931" alt="UAM7" src="https://github.com/user-attachments/assets/db30a9f5-6775-4a1f-a77d-ec8ad60dede6" />


## Step 3 — Configured User Properties
After creating each account, opened the user properties and filled in the Organization tab with each user's Job Title and Department. Maintaining accurate user attributes is important for reporting, email directory listings, and access management in real environments.

| User | Job Title | Department |
|---|---|---|
| jane.doe | HR Manager | HR |
| john.smith | Help Desk Technician | IT |
| tom.wilson | Systems Administrator | IT |
| sarah.jones | Accountant | Finance |
| mike.brown | Sales Rep | Sales |
| lisa.davis | HR Coordinator | HR |

<img width="1228" height="879" alt="UAM6" src="https://github.com/user-attachments/assets/25acdc8b-8dd7-469f-888f-f34210130d18" />


## Step 4 — Created Security Groups
Created a Security Group for each department and assigned users to their appropriate group. Security groups are used to manage resource access across the domain — a user's group membership determines what shared drives, printers, and applications they can access without needing to configure permissions per individual user.

Groups created:

| Group Name | OU | Members |
|---|---|---|
| GRP_IT_Users | IT | john.smith, tom.wilson |
| GRP_Finance_Users | Finance | sarah.jones |
| GRP_HR_Users | HR | jane.doe, lisa.davis |
| GRP_Sales_Users | Sales | mike.brown |

<img width="1224" height="930" alt="UAM9" src="https://github.com/user-attachments/assets/1fefdcb4-5ffc-45bb-a999-db823ae86b50" />
<img width="1222" height="928" alt="UAM10" src="https://github.com/user-attachments/assets/fae4a3dc-5a16-4c2a-9674-e33a6759f1f4" />

## Step 5 — Configured Password Policy GPO
Created a Group Policy Object to enforce password security standards across all domain user accounts. Linked the GPO at the domain level so it applies universally to every user in corp.local.

<img width="1223" height="933" alt="UAM12" src="https://github.com/user-attachments/assets/01508619-bcd0-486e-8218-651ea7bf6627" />


| Setting | Value |
|---|---|
| Minimum password length | 10 characters |
| Password complexity requirements | Enabled |
| Maximum password age | 90 days |
| Minimum password age | 1 day |
| Enforce password history | 5 passwords remembered |

<img width="1226" height="931" alt="UAM14" src="https://github.com/user-attachments/assets/ffb20191-6452-4927-a1a0-305bffc95f78" />


## Step 6 — Configured Account Lockout Policy GPO
Created a separate GPO to protect domain accounts against brute force login attempts. After 5 failed login attempts the account is locked for 15 minutes before automatically resetting, balancing security with user convenience.

<img width="1224" height="928" alt="UAM17" src="https://github.com/user-attachments/assets/caff56c0-c71a-48dc-b18b-6aa4c893c5fc" />

<br>
<br>

Linked both Group Policy Objects to Domain

<br>

<img width="617" height="536" alt="UAM20" src="https://github.com/user-attachments/assets/9bbae53a-a3bb-44bb-bdba-158593ba1426" />


## Step 7 — Account Offboarding Procedure
Performed a complete offboarding workflow on john.smith to simulate a employee termination. Following security best practice, the account was disabled rather than deleted — preserving the audit trail and allowing recovery if needed.

Offboarding steps performed in order:

**1. Disabled the account**
<br>

Right-clicked the user in ADUC → Disable Account. The account icon
shows a downward arrow confirming it is disabled and cannot be
used to log in.

<img width="1226" height="929" alt="UAM21" src="https://github.com/user-attachments/assets/194dbd23-b292-41f7-8332-c78e5e907bfa" />

<br>


**2. Removed from all security groups**
<br>

Opened user properties → Member Of tab → removed from GRP_IT_Users and any other group memberships. This immediately revokes access to all resources the groups controlled.


<img width="1226" height="928" alt="UAM22" src="https://github.com/user-attachments/assets/b600a96f-f4be-4905-9503-1e9a59455835" />

<br>

**3. Moved to Disabled_Accounts OU**
<br>

Right-clicked the user → Move → selected Disabled_Accounts OU. Keeping disabled accounts in a dedicated OU makes auditing and reviewing terminated accounts straightforward.

<img width="1228" height="927" alt="UAM23" src="https://github.com/user-attachments/assets/3ec899e2-ff5d-4a7b-a588-531a46532029" />

<br>

**4. Added termination note to account description**
<br>

Opened user properties → General tab → added a description noting the date and reason for disabling. This provides a clear record directly on the account for future reference.

<img width="1226" height="929" alt="UAM24" src="https://github.com/user-attachments/assets/fbc7c8bc-56b7-4ede-aa89-f26a80d39c08" />

## Outcome
Successfully demonstrated the full Active Directory user lifecycle from account creation through offboarding. Configured department-based Organizational Units, created and organized user accounts, managed security group memberships, enforced domain-wide security policies through Group Policy, and performed a complete account offboarding following security best practices.

## Lessons Learned

