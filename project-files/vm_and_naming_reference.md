# VM Inventory
| VM Name | Role | VLAN | IP Address | OS | Notes |
|---|---|---|---|---|---|
| PT-OPNSENSE-01 | Firewall/Router | WAN + LAN/VLANs | WAN: DHCP  LAN: 172.16.x.1 | OPNsense | Routes lab networks |
| PT-DC-01 | Domain Controller | VLAN 10 | 172.16.10.10 | Windows Server 2025 Standard Evaluation | AD DS, DNS |
| PT-PC-01 | Workstation | VLAN 20 | DHCP / 172.16.20.188 | Windows 11 Enterprise Evaluation | Domain-joined client |

# Naming Conventions

## Domain
corp.polktech.local

## Server Names
- PT-DC-01
- PT-FS-01
- PT-OPNSENSE-01

## Workstation Names
- PT-PC-01
- PT-PC-02

## User Naming
Format:
firstinitial.lastname

Example:
k.polk

## Admin Accounts
Format:
adm-firstinitial.lastname

Example:
adm-k.polk
