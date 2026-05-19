# VM Inventory
| VM Name | Role | VLAN | IP Address | OS | Notes |
|---|---|---|---|---|---|
| PT-OPNSENSE-01 | Firewall/Router | WAN + LAN/VLANs | WAN: DHCP  LAN: 172.16.x.1 | OPNsense | Routes lab networks |
| PT-DC-01 | Domain Controller | VLAN 10 | 172.16.10.10 | Windows Server 2022 | AD DS, DNS, DHCP |
| PT-WIN11-01 | Workstation | VLAN 20 | DHCP | Windows 11 Pro | Domain-joined client |

# Naming Conventions

## Domain
corp.polktech.local

## Server Names
- PT-DC-01
- PT-FS-01
- PT-OPNSENSE-01

## Workstation Names
- PT-WIN11-01
- PT-WIN11-02

## User Naming
Format:
first.last

Example:
jane.smith

## Admin Accounts
Format:
adm-first.last

Example:
adm-kerron.polk
