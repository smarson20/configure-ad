
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>






# On-Premises Active Directory Deployed in Azure (Cloud Lab)

Welcome to this comprehensive and hands-on guide for setting up and managing a traditional-style Active Directory (AD) environment within Microsoft Azure. Whether you're studying for certifications, upskilling, or simulating enterprise IT infrastructure, this lab bridges the gap between on-premises setups and modern cloud capabilities.

---

## Technologies & Tools Used
- **Microsoft Azure** — Acts as the foundation of our cloud infrastructure. Virtual Machines and networking services replicate an enterprise-grade environment.
- **Remote Desktop Protocol (RDP)** — Enables remote access and management of virtual machines.
- **Active Directory Domain Services (AD DS)** — Core identity management and domain service used to simulate on-prem AD.
- **Windows PowerShell & PowerShell ISE** — Used for automation and administrative scripting throughout the lab.

##  Operating Systems Deployed
- **Windows Server 2022** — Hosts the domain controller; delivers all directory and domain services.
- **Windows 10 Pro (22H2)** — Acts as a domain-joined client machine for testing logins and policies.

---

## High-Level Lab Overview
1. **Provision Azure Infrastructure** — Lay down the cloud networking and compute foundation.
2. **Deploy and Configure Active Directory** — Install domain services and create a domain.
3. **Automate Bulk User Creation** — Use PowerShell to scale identity provisioning.
4. **Apply Policies & Manage Accounts** — Implement GPOs and perform routine AD tasks.

---

## 🔧 Part 1: Infrastructure Setup in Azure

###  Domain Controller Setup (`DC-1`)
- Set up a **Resource Group** to organize lab assets.
- Create a **Virtual Network** and **Subnet** for secure communication.
- Deploy a **Windows Server 2022 VM** named `DC-1`.
- Assign a **Static Private IP** to ensure reliable DNS resolution.
- Temporarily **disable the firewall** for setup purposes (be sure to re-enable for production scenarios).

### Client VM Setup (`Client-1`)
- Deploy a **Windows 10 Pro VM** named `Client-1`.
- Ensure it’s connected to the **same subnet** as `DC-1`.
- Manually configure **DNS settings** to point to `DC-1`'s IP.
- Validate connectivity:
  - Run `ping DC-1` to test reachability.
  - Use `ipconfig /all` to confirm DNS configuration.

---

## Part 2: Deploying Active Directory

### Install AD DS on `DC-1`
1. Connect to `DC-1` using RDP.
2. Install the **Active Directory Domain Services** role.
3. Promote the server to a **Domain Controller** and create a new forest: `mydomain.com`.
4. Reboot and log in using domain credentials: `mydomain\labuser`.

### Create a Domain Admin Account
1. Launch **Active Directory Users and Computers (ADUC)**.
2. Organize the directory by creating Organizational Units (OUs):
   - `_EMPLOYEES`
   - `_ADMINS`
3. Create a user account for `Jane Doe`:
   - Username: `jane_admin`
   - Secure password: `Cyberlab123!`
4. Grant Jane elevated privileges by adding her to the **Domain Admins** group.
5. Confirm login works with: `mydomain\jane_admin`.

###  Join Client-1 to Domain
1. Access `Client-1` using the local admin account.
2. Join the domain: `mydomain.com`.
3. In ADUC, create another OU called `_CLIENTS` and move `Client-1` into it for clarity and future GPO targeting.

---

## Part 3: PowerShell User Automation

### Enable Remote Desktop on Client-1
1. Log into `Client-1` as `jane_admin`.
2. Enable **Remote Desktop**.
3. Modify permissions to allow **Domain Users** to connect remotely.

### Bulk User Creation with Script
1. Log into `DC-1` as `jane_admin`.
2. Open **PowerShell ISE as Administrator**.
3. Run the prebuilt script:
   - [Generate-Names-Create-Users.ps1](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
4. Review the newly created accounts in the `_EMPLOYEES` OU.
5. Choose one of the users and test a domain login on `Client-1`.

---

## Part 4: Group Policy & Account Management

### Configure Account Lockout Policy
1. Open **Group Policy Management Console** on `DC-1`.
2. Edit the **Default Domain Policy**.
3. Set lockout threshold:
   - `5` failed login attempts before lockout.
4. Simulate login failures to trigger the policy.
5. Use ADUC to unlock the account and reset the password.

### Enable/Disable User Accounts
1. Disable a user account in **ADUC**.
2. Attempt to log in — access should be denied.
3. Re-enable the account and verify successful login.

### Audit User Activity with Event Viewer
1. Open **Event Viewer** on both `DC-1` and `Client-1`.
2. Navigate to **Security Logs**.
3. Look for key events:
   - Successful and failed logins
   - Account changes
   - Lockouts

---

##  Summary
This lab experience demonstrates how to design and deploy a traditional Active Directory environment entirely in the cloud. By using Azure and Windows-based services, you gain hands-on practice in configuring domain controllers, automating identity provisioning, applying policies, and auditing user behavior — all critical skills which will help to refine your knowledge of  enterprise  domain management.




