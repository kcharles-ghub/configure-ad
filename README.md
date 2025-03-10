<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>


# Active Directory Installation and Configuration Tutorial

This tutorial guides you through installing and configuring Active Directory on Azure. We will create a Domain Controller and a Client VM, ensure connectivity, and set up Active Directory with user accounts.

## Environments and Technologies Used
- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

## Operating Systems Used
- Windows Server 2022
- Windows 10 (22H2)

## Table of Contents
- [Setup Resources in Azure](#setup-resources-in-azure)
- [Ensure Connectivity between the Client and Domain Controller](#ensure-connectivity-between-the-client-and-domain-controller)
- [Install Active Directory](#install-active-directory)
- [Create Admin and Normal User Accounts in AD](#create-admin-and-normal-user-accounts-in-ad)
- [Join Client-1 to Your Domain](#join-client-1-to-your-domain)
- [Setup Remote Desktop for Non-Administrative Users on Client-1](#setup-remote-desktop-for-non-administrative-users-on-client-1)
- [Create Additional Users](#create-additional-users)

  

## Setup Resources in Azure

1. **Create the Domain Controller VM (Windows Server 2022) named `DC-1`**
   - Take note of the Resource Group and Virtual Network (Vnet) that get created at this time.
   - Set the Domain Controller’s NIC Private IP address to be static.

2. **Create the Client VM (Windows 10) named `Client-1`**
   - Use the same Resource Group and Vnet created in Step 1.
   - Ensure both VMs are in the same Vnet (you can check the topology with Network Watcher).

## Ensure Connectivity between the Client and Domain Controller

1. **Login to `Client-1` with Remote Desktop**
   - Ping `DC-1`’s private IP address with `ping -t <ip address>` (perpetual ping).

2. **Login to the Domain Controller**
   - Enable ICMPv4 on the local Windows Firewall.

3. **Check back at `Client-1` to see the ping succeed**

## Install Active Directory

1. **Login to `DC-1` and install Active Directory Domain Services**
2. **Promote as a DC**
   - Set up a new forest as `mydomain.com` (it can be anything; just remember what it is).
   - Restart and log back into `DC-1` as user: `mydomain.com\labuser`.

## Create Admin and Normal User Accounts in AD

1. **In Active Directory Users and Computers (ADUC)**
   - Create an Organizational Unit (OU) called `_EMPLOYEES`.
   - Create a new OU named `_ADMINS`.
   - Create a new employee named `Jane Doe` (same password) with the username `jane_admin`.
   - Add `jane_admin` to the `Domain Admins` Security Group.
   - Log out/close the Remote Desktop connection to `DC-1` and log back in as `mydomain.com\jane_admin`.
   - Use `jane_admin` as your admin account from now on.

## Join Client-1 to Your Domain

1. **From the Azure Portal**
   - Set `Client-1`’s DNS settings to the DC’s Private IP address.
   - Restart `Client-1`.

2. **Login to `Client-1` (Remote Desktop) as the original local admin (labuser)**
   - Join it to the domain (computer will restart).

3. **Login to the Domain Controller (Remote Desktop)**
   - Verify that `Client-1` shows up in Active Directory Users and Computers (ADUC) inside the `Computers` container on the root of the domain.
   - Create a new OU named `_CLIENTS` and drag `Client-1` into it (this step is not necessary; it is just for organizational purposes).

## Setup Remote Desktop for Non-Administrative Users on Client-1

1. **Log into `Client-1` as `mydomain.com\jane_admin`**
   - Open system properties.
   - Click “Remote Desktop”.
   - Allow “domain users” access to remote desktop.

2. **You can now log into `Client-1` as a normal, non-administrative user**
   - (Normally, you’d want to do this with Group Policy, which allows you to change MANY systems at once.)

## Create Additional Users

1. **Login to `DC-1` as `jane_admin`**
2. **Open PowerShell_ise as an administrator**
   - Create a new File and paste the contents of the script into it ([Generate-Names-Create-Users.ps1](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)).
   - Run the script and observe the accounts being created.

3. **When finished, open ADUC and observe the accounts in the appropriate OU**
   - Attempt to log into `Client-1` with one of the accounts (take note of the password in the script).
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
