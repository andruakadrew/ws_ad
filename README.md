# Windows Server AD Lab — “SkateShop” Domain

A homelab Active Directory environment built on VirtualBox that simulates a small retail company (Retail, Warehouse, Management). I used OUs, security groups, GPOs, and Group Policy Preferences to automate user experience and enforce least privilege.

## Goals
- Practice real-world AD tasks (OUs, users, groups, GPOs, permissions).
- Automate drive mappings, printer deployment, and folder redirection.
- Demonstrate documentation, scripting, and reproducibility.

## Topology
- **Domain:** skateshop.local
- **DC:** WIN-DC01 (AD DS, DNS)
- **Clients:** WIN10-Retail01, WIN10-Warehouse01
- **Network:** Host-only + NAT; DHCP (scope plan in `configs/dhcp/`)

! [OU Structure]

## Key Features Implemented
- Departmental OUs with delegated controls
- Security groups and targeted GPOs:
  - Retail lockdown (no Task Manager/Device Manager)
  - Drive mapping: `Z:` → `\\SkateShop\RetailDocs`
  - Printers by OU (Retail gets “Store Printer”)
- Folder Redirection (Documents/Desktop) for roaming-like experience
- Baseline password and account lockout policies
- Auditing for logon and object access

## How to Recreate (High Level)
1. Build VMs (see `provisioning/virtualbox/`).
2. Promote DC & configure DNS (`provisioning/bootstrap/dc_prep.ps1`).
3. Create OUs/users/groups (`scripts/powershell/New-OU-SkateShop.ps1`, `New-Users-Bulk.ps1`).
4. Apply GPO baselines and GPP items (`scripts/powershell/Set-GPO-Baseline.ps1`, `Set-DriveMappings-GPP.ps1`, `Deploy-Printers-GPP.ps1`).
5. Join clients to domain (`provisioning/bootstrap/client_join.ps1`).
6. Test scenarios (`demonstrations/`).

## Results (Screenshots)
- Drive mapping works on Retail login 
- Printer auto-appears for Retail OU 
- Restricted tools blocked for Retail 
- Folder redirection stored to file server 

## Scripts
- PowerShell automation lives in `/scripts/powershell/` with comments and idempotent checks.
- Exports and redacted inventories in `/configs/ad_exports/`.

## What I Learned
- Designing OU/GPO strategy by department and role
- NTFS vs Share permissions in practice
- Targeting with security filtering and WMI (where useful)
- Troubleshooting GPO precedence and Resultant Set of Policy (RSoP)

## Notes on Data & Privacy
This repo uses **redacted/synthetic data** only (see `SECURITY.md`).

