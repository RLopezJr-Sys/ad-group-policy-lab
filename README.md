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

---

## Key Takeaways & Troubleshooting
* **Security Filtering Nuances:** Discovered that user-configuration GPOs targeting a specific user may also require the client computer object to have read permissions within security filtering for policies to process cleanly.
* **Command Line Tools:** Utilized `gpupdate /force` for forcing policy replication and `gpresult` for auditing applied policies.
