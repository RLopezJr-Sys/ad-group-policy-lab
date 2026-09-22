# ad-group-policy-lab
Hands-on Active Directory Group Policy management, security filtering, and troubleshooting lab.

# Active Directory Group Policy (GPO) Lab

Hands-on Active Directory Group Policy management, security filtering, and troubleshooting lab built on an Azure-deployed hybrid environment.

## Lab Architecture
* **Domain Controller (`172.16.0.4`):** Hosts the Active Directory Domain Services (`lab.local`) and the Group Policy Management Console (`gpmc.msc`).
* **Client VM (`rg-test-machine`):** Joined to the domain to receive, update, and test deployed Group Policy Objects.

---

## Objectives & Tasks Covered

### 1. Computer Configuration: Disabling Domain Firewall
* **Objective:** Enforce a computer-level policy to turn off the Windows Defender Firewall for the domain profile.
* **Path:** `Computer Configuration > Policies > Windows Settings > Security Settings > Windows Defender Firewall`
* **Verification:** Ran `gpupdate /force` and `gpresult /r /scope computer` on the client VM to confirm application.

### 2. User Configuration: Restricting Control Panel Access
* **Objective:** Enforce a user-level policy to prohibit access to the Control Panel and PC settings.
* **Path:** `User Configuration > Policies > Administrative Templates > Control Panel`
* **Security Filtering:** Configured to target specific test user accounts (e.g., `Tom Brady`) and scoped computer objects to ensure correct permission propagation.
* **Verification:** Logged into the client VM using domain user credentials, verified policy inheritance, and confirmed access was successfully blocked.

# Active Directory Lab Project: Group Policy Object (GPO) Configuration

## Objective
Configure and deploy a custom Group Policy Object (GPO) to restrict specific user actions (blocking access to the Control Panel and PC settings) for an isolated test user within an Active Directory environment, followed by verification and troubleshooting.

---

## Step-by-Step Implementation

### 1. Environment & Infrastructure Overview
* **Domain Controller**: `172.16.0.4` (`lab.local`)
* **Client Workstation**: `rg-test-machine`
* **Test User**: `Mike Smith`

### 2. Creating and Scoping the Test GPO
* Opened **Group Management Console** on the Domain Controller.
* Created a new GPO named `TEST GPO` and linked it to the appropriate Organizational Unit (OU).
* Configured **Security Filtering** to isolate the policy scope:
  * Removed the default **Authenticated Users** group to prevent broad application.
  * Added the specific test user `Mike Smith` so the policy only targets his account.

### 3. Configuring Policy Settings
* Opened the **Group Policy Management Editor** for `TEST GPO`.
* Navigated to the policy path: 
  * `User Configuration > Policies > Administrative Templates > Control Panel`
* Located the setting **"Prohibit access to Control Panel and PC settings"**.
* Configured the setting to **Enabled**, reviewed the built-in configuration documentation detailing the blocking of `Control.exe` and `SystemSettings.exe`, and saved the changes.

### 4. Troubleshooting & Encountered Roadblocks (Portfolio Highlight)
* **Issue**: While attempting to log into the client workstation (`rg-test-machine`) as `Mike Smith` to test the GPO, a Remote Desktop connection error occurred: *"The connection was denied because the user account is not authorized for remote login."*
* **Root Cause**: By default, standard domain users lack explicit remote login permissions on Windows client workstations unless granted via local security policies or group membership.

# Active Directory Lab Project: Group Policy Object (GPO) Configuration

## Objective
Configure and deploy a custom Group Policy Object (GPO) to restrict specific user actions (blocking access to the Control Panel and PC settings) for an isolated test user within an Active Directory environment, followed by verification and troubleshooting.

---

## Step-by-Step Implementation

### 1. Environment & Infrastructure Overview
* **Domain Controller**: `172.16.0.4` (`lab.local`)
* **Client Workstation**: `rg-test-machine`
* **Test User**: `Mike Smith`

### 2. Creating and Scoping the Test GPO
* Opened **Group Management Console** on the Domain Controller.
* Created a new GPO named `TEST GPO` and linked it to the appropriate Organizational Unit (OU).
* Configured **Security Filtering** to isolate the policy scope.
  * Removed the default **Authenticated Users** group to prevent broad application.
  * Added the specific test user `Mike Smith` so the policy only targets his account.

<img width="1089" height="729" alt="isolated security test user" src="https://github.com/user-attachments/assets/151c4c9f-9255-4d8a-bb7e-da0c631e74c1" />


### 3. Configuring Policy Settings
* Opened the **Group Policy Management Editor** for `TEST GPO`.
* Navigated to the policy path: 
  * `User Configuration > Policies > Administrative Templates > Control Panel`
* Located the setting **"Prohibit access to Control Panel and PC settings"**.

* Configured the setting to **Enabled**, reviewed the built-in configuration documentation detailing the blocking of `Control.exe` and `SystemSettings.exe`, and saved the changes.

<img width="847" height="797" alt="applied a policy setting to a user" src="https://github.com/user-attachments/assets/69410533-f74d-4e6a-b38c-bf8f2b04b1b7" />


### 4. Troubleshooting & Encountered Roadblocks (Portfolio Highlight)
* **Issue**: While attempting to log into the client workstation (`rg-test-machine`) as `Mike Smith` to test the GPO, a Remote Desktop connection error occurred: *"The connection was denied because the user account is not authorized for remote login."*

<img width="668" height="151" alt="error" src="https://github.com/user-attachments/assets/7d793c54-b029-4031-8981-f60a15b12b29" />


* **Root Cause**: By default, standard domain users lack explicit remote login permissions on Windows client workstations unless granted via local security policies or group membership.
---

## Key Takeaways & Troubleshooting
* **Security Filtering Nuances:** Discovered that user-configuration GPOs targeting a specific user may also require the client

* computer object to have read permissions within security filtering for policies to process cleanly.
* **Command Line Tools:** Utilized `gpupdate /force` for forcing policy replication and `gpresult` for auditing applied policies.

### 4. Troubleshooting & Encountered Roadblocks (Portfolio Highlight)
* **Issue 1 (RDP Authorization Block)**: While attempting to log into the client workstation (`rg-test-machine`) as `Mike Smith` to test the GPO, a Remote Desktop connection error occurred stating the account was not authorized for remote login.
  * **Root Cause**: Standard domain users lack explicit remote login permissions by default.
  * **Resolution**: Added `Mike Smith` to the **Remote Desktop Users** group in Active Directory Users and Computers (ADUC) and executed `gpupdate /force` on the client.

<img width="1437" height="790" alt="access denied" src="https://github.com/user-attachments/assets/7965a89d-ace0-43cf-900e-176e1dd62a8b" />

* **Issue 2 (GPO Verification)**: Once successfully authenticated, attempted to open the Control Panel as `Mike Smith` to verify policy enforcement.
  * **Resolution/Result**: The system successfully intercepted the request and displayed the "Access Denied" restriction message, confirming the GPO was actively blocking `Control.exe` and `SystemSettings.exe` as configured.
 
## Conclusion & Key Takeaways

This project successfully demonstrated the implementation, deployment, and troubleshooting of a custom Group Policy Object (GPO) in an active Windows Server domain environment. By isolating the policy scope using Security Filtering for `Mike Smith` and enforcing the **"Prohibit access to Control Panel and PC settings"** administrative template, unauthorized user modifications were successfully prevented. 

Additionally, navigating and resolving the real-world RDP authorization hurdles—specifically managing ADUC group memberships and User Rights Assignments for Remote Desktop Services—highlighted critical troubleshooting skills required for enterprise IT administration and desktop support roles. The resulting configuration provides a solid framework for maintaining baseline security compliance across client workstations.
