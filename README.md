Active Directory Home Lab — Phase 1: Environment Build & IT Administration
Domain: IT Administration | Active Directory | Help Desk Operations | Group Policy
Tools: Windows Server 2022 · Active Directory Domain Services · Group Policy Management Console · Active Directory Users and Computers
Difficulty: Intermediate
Phase: 1 of 2 (Phase 2 — Attack & Detection coming separately)
Date Completed: May 27, 2026


Project Overview
This project builds a fully functional Active Directory environment from scratch in a personal VirtualBox lab, simulating the identity and access management infrastructure of a real enterprise environment. The fictional organization, Apex Defense Solutions, is a government contractor operating an internal domain (apexdefense.local) with departmental structure, domain users, security groups, and Group Policy enforcement.

Phase 1 covers the build and administration layer: Windows Server 2022 installation, Domain Controller promotion, organizational structure creation, and five help desk scenarios executed against the live domain. Phase 2 will introduce a Windows 10 workstation and Kali Linux attacker VM to simulate common Active Directory attack techniques and document their detection in Windows Event Logs.


Environment
Component
Details
Host Machine
Personal PC, Windows, 32GB DDR5, PCIe 3.0 SSD
Virtualization
Oracle VirtualBox
Domain Controller
DC01 Windows Server 2022 Standard Evaluation (Desktop Experience)
DC01 Resources
4096MB RAM, 2 vCPUs, 60GB VDI
Network
VirtualBox Internal Network (apexdefense-lab) isolated, no internet
DC01 Static IP
192.168.1.10
DNS
127.0.0.1 (self-referencing required for AD-integrated DNS)
Domain
apexdefense.local
NetBIOS Name
APEXDEFENSE
Forest/Domain Level
Windows Server 2016



What Was Built
Active Directory Structure
Organizational Units:

apexdefense.local

├── _DEPARTMENTS

│   ├── IT

│   ├── HR

│   ├── Finance

│   └── Management

└── _DISABLED_ACCOUNTS

Domain Users (9 total):

Name
Username
Department
Group
John Carter
jcarter
IT
IT-Admins
Sarah Mitchell
smitchell
IT
IT-Admins
James Reynolds
jreynolds
HR
HR-Staff
Lisa Thompson
lthompson
HR
HR-Staff
Michael Torres
mtorres
Finance
Finance-Team
Angela Brooks
abrooks
Finance
Finance-Team
David Hawkins
dhawkins
Management
Management-Team, Finance-Team
Rachel Kim
rkim
Management
Management-Team
Kevin Park
kpark
_DISABLED_ACCOUNTS
IT-Admins (disabled -  terminated)


Security Groups: IT-Admins · HR-Staff · Finance-Team · Management-Team


Group Policy Objects
Apex-Password-Policy (Domain-Wide)

Password history: 10 passwords
Maximum password age: 90 days
Minimum password age: 1 day
Minimum password length: 12 characters
Complexity requirements: Enabled

Account Lockout Policy (Domain-Wide)

Lockout threshold: 5 invalid attempts
Lockout duration: 30 minutes
Reset counter after: 30 minutes

Apex-Desktop-Restrictions (HR and Finance OUs)

Prevent changing desktop background: Enabled
Prevent changing screen saver: Enabled
Prevent access to command prompt: Enabled
Remove Run menu from Start Menu: Enabled


Help Desk Scenarios
Five real-world help desk scenarios were simulated against the live domain each representing a common IT support ticket type:

Scenario 1 — New User Onboarding Created a new domain account for Kevin Park in the IT OU and added him to the IT-Admins security group. Performed from memory without step-by-step guidance.

Scenario 2 — Password Reset Reset Lisa Thompson's password with forced change at next logon. Identified and resolved a Password never expires conflict that prevented the forced change option from being available documented as a key lesson about conflicting account settings.

Scenario 3 — Account Lockout Investigation Investigated Michael Torres's account status using the Account tab in ADUC. Distinguished between locked out, disabled, and misconfigured account states. Applied remediation including password reset and account unlock.

Scenario 4 — Cross-Department Permissions Change Added David Hawkins to the Finance-Team security group following a promotion, while his user account remained in the Management OU. Demonstrates the separation between OU location and group membership a fundamental Active Directory concept.

Scenario 5 — Terminated Employee Disabled Kevin Park's account immediately upon termination notification and moved it to the _DISABLED_ACCOUNTS holding OU. Mirrors real enterprise termination procedure: disable same day, retain 90 days for audit, then permanently delete.


Key Technical Concepts Demonstrated
AD DS role installation and Domain Controller promotion — building a new forest from scratch
OU design — using underscore prefix convention, protecting containers from accidental deletion, separating department structure from disabled accounts
Group membership vs. OU location — users live in OUs but access is controlled by group membership; these are independent
GPO scope — domain-wide policies vs. OU-scoped policies, inheritance flow from parent to child OUs
AD-integrated DNS — why a DC must point to itself (127.0.0.1) for DNS rather than an external resolver
SYSVOL — the replicated share that distributes GPOs and login scripts across all Domain Controllers
Account settings conflicts — Password never expires and User must change password at next logon cannot coexist
Termination workflow — disable, move, retain, then delete with audit trail reasoning


Phase 2 Preview
Phase 2 will add a Windows 10 domain-joined workstation and Kali Linux attacker VM, then execute common Active Directory attack techniques against the apexdefense.local domain:

Attack
Tool
What It Does
LLMNR Poisoning
Responder
Captures NTLMv2 hashes from broadcast requests
Kerberoasting
Impacket
Requests and cracks service account tickets offline
Pass-the-Hash
Impacket
Authenticates using hash without plaintext password
BloodHound Enumeration
BloodHound + SharpHound
Maps attack paths to Domain Admin visually


Each attack will be documented with exploitation steps, Windows Event Log detection signatures, and prevention controls.


Skills Demonstrated
Windows Server 2022 installation and configuration from scratch
Active Directory Domain Services role installation and DC promotion
Static IP and AD-integrated DNS configuration
Organizational Unit design following enterprise conventions
Domain user and security group creation and management
Group Policy Object creation and scoped linking
Password policy and account lockout policy configuration
User environment restriction via Administrative Templates
Help desk operations in a domain environment onboarding, password resets, lockout investigation, permissions changes, termination workflow


Repository Structure
active-directory-home-lab/

│

├── README.md                              ← This file

│

├── Phase1/

│   ├── Bryan_Sykes_AD_Lab_Phase1.docx    ← Full Phase 1 documentation

│   └── screenshots/

│       ├── 01-windows-server-2022-installed.png

│       ├── 02-server-manager-dashboard.png

│       ├── 03-dc01-renamed-static-ip.png

│       ├── 04-ad-ds-role-installed.png

│       ├── 05-ad-ds-promotion-complete.png

│       ├── 06-server-manager-post-promotion.png

│       ├── 07-ad-tools-menu.png

│       ├── 08-active-directory-users-computers.png

│       ├── 09-organizational-units-created.png

│       ├── 10-domain-users-created.png

│       ├── 11-security-groups-created.png

│       ├── 12-users-added-to-groups.png

│       ├── 13-password-policy-gpo.png

│       ├── 14-account-lockout-policy-gpo.png

│       ├── 15-gpo-desktop-restrictions-hr.png

│       ├── 16-helpdesk-new-user-onboarding.png

│       ├── 17-helpdesk-password-reset.png

│       ├── 18-helpdesk-account-lockout-resolution.png

│       ├── 19-helpdesk-permissions-change.png

│       └── 20-helpdesk-terminated-employee.png

│

└── Phase2/                                ← Coming soon

    ├── Bryan_Sykes_AD_Lab_Phase2.docx

    └── screenshots/


Notes
Windows Server 2022 evaluation ISO from Microsoft Evaluation Center 180-day free license
All VMs run on an isolated VirtualBox Internal Network with no internet access
apexdefense.local is a fictional domain no real organizational data involved
Phase 1 snapshot taken after completion as a rollback point before Phase 2 attack simulation



Part of an ongoing cybersecurity portfolio. Additional projects cover AWS GuardDuty cloud threat detection, OWASP Top 10 web application exploitation, AI model supply chain risk assessment, Salesforce vendor risk assessment, and Security Onion SIEM detection engineering.

