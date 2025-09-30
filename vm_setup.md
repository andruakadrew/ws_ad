# Virtual Machine and Windows Server Active Directory Installation steps:

## Get the Windows Server ISO
https://www.microsoft.com/en-us/evalcenter/

## Create a VM (Virtual Machine) in VirtualBox
- Open Virtual Box > Click New > Name: _WIN-DC01_
- Choose **Type**: Microsoft Windows, Version: Windows Server (64-bit).
- Allocate suffiecient resources (at least 4 GB RAM, 2 CPU'S, and ~50 GB disk).
- Add _Two_ Network Adapters: Adapter 1: NAT (for internet access) Adapter 2: Host-Only (to simulate internal LAN).
- Mount the Windows Server ISO in the Optical Drive.

## Install Windows Server
- Boot the VM and boot from the ISO.
- Select Windows Server (Desktop Experience) edition.
- Create a Local Administrator password during setup.
- Once installed, log in as Administrator.

## Prepare the Server
- Assign a static IP on the Host-Only Adapter: (example, _192.168.10.10_, gateway _192.168.10.1_).
- Rename the computer (example, _WIN-DC01_).
- Install _Guest Additions_ in VirtualBox (helps with resizing windows).

## Install AD & DNS
- Open Server Manager > Manage > Add Roles and Features.
- Select Active Directory Domain Services (AD DS).
- After install, click _Promote this server to a domain controller_.
- Create a new forest (example, _Root Domain = <domain_name>.local, Set DSRM password_).
- Reboot into the new Domain.

## Verify Domain Controller
- Log in as _<domain_name>/Administrator_.
- Open Server Manager > Tools > Active Directory Users and Computers (ADUC).
- Verify the domain tree <domain_name>.local exist.
- Check DNS Manager, ensure foward lookup zone for <domain_name>.local is created.

## Add Organizational Units
- IN ADUC, create Organizational Units (OU): **Retail, Warehouse, Sales**.

## Create Users & Groups
- Create sample users (example, _retail_employee_1, warehouse_employee_3, sales_employee_2_)
- Assign those new users into their respective OU

