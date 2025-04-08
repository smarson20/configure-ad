# In-House Active Directory Deployed in the Cloud (Azure)

This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.

## Environments and Technologies Used
- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

## Operating Systems Used
- Windows Server 2022
- Windows 10 (22H2)

## High-Level Deployment and Configuration Steps
- Preparing the AD Infrastructure in Azure
- Deploying Active Directory
- Creating Users with PowerShell
- Group Policy and Managing Accounts

---

## Part 1: Preparing the AD Infrastructure in Azure

### Setup Domain Controller in Azure
- Create a Resource Group in Azure.
- Set up a Virtual Network and Subnet.
- Create the Domain Controller VM (`DC-1`) with Windows Server 2022.
- Assign a Static Private IP to `DC-1`.
- Disable Windows Firewall on `DC-1` for testing purposes.

### Setup Client-1 in Azure
- Create a Client VM (`Client-1`) with Windows 10.
- Ensure it's in the same Virtual Network and Subnet as `DC-1`.
- Update DNS settings on `Client-1` to point to `DC-1`'s IP.
- Test connectivity with `ping` and verify DNS with `ipconfig /all`.

---

## Part 2: Deploying Active Directory

### Install Active Directory on DC-1
1. Log in to `DC-1`.
2. Install Active Directory Domain Services (AD DS).
3. Promote `DC-1` to a Domain Controller, creating a new forest (e.g., `mydomain.com`).
4. Restart and log in as `mydomain.com\labuser`.

### Create a Domain Admin User
1. Open ADUC.
2. Create OUs: `_EMPLOYEES` and `_ADMINS`.
3. Add a new user `Jane Doe` with username `jane_admin`, password `Cyberlab123!`.
4. Add `jane_admin` to `Domain Admins` group.
5. Log in as `mydomain.com\jane_admin`.

### Join Client-1 to the Domain
1. Log into `Client-1` as local admin.
2. Join to `mydomain.com`.
3. Create OU `_CLIENTS` in ADUC and move `Client-1` into it.

---

## Part 3: Creating Users with PowerShell

### Enable Remote Desktop
1. On `Client-1`, log in as `jane_admin`.
2. Enable Remote Desktop and allow access for Domain Users.

### Create Users via PowerShell
1. On `DC-1`, log in as `jane_admin`.
2. Open PowerShell ISE as admin.
3. Use the script [Generate-Names-Create-Users.ps1](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1).
4. Verify users appear in `_EMPLOYEES` OU.
5. Log into `Client-1` using one of the new accounts.

---

## Part 4: Group Policy and Managing Accounts

### Configure Account Lockout Policy
1. Open Group Policy Management on `DC-1`.
2. Edit Default Domain Policy:
   - Set Account Lockout Threshold to 5.
3. Test login failures.
4. Unlock account in ADUC and reset password.

### Enable/Disable Accounts
1. Disable a user in ADUC.
2. Attempt login and note failure.
3. Re-enable the account and confirm successful login.

### Event Viewer Logs
1. Open Event Viewer (`eventvwr.msc`) on `DC-1` and `Client-1`.
2. Check Security logs for authentication and account activity.

---

This guide provides a foundational approach to implementing and managing Active Directory within a cloud-based infrastructure, simulating an on-premises environment using Azure.

