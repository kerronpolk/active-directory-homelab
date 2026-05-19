# IP Address Plan

## Home LAN

| Device | IP | Notes |
|---|---|---|
| Xfinity Gateway | 10.0.0.1 | Home router and DHCP server |
| Proxmox Host (vmbr0) | 10.0.0.2 | Static, set during Proxmox install |
| OPNsense WAN | DHCP from Xfinity Gateway | Static reservation planned when LAN is reconfigured |
| User/Host PC | DHCP from Xfinity Gateway | 10.0.0.x |

## PolkTech Lab Networks

| Network | VLAN | Subnet | Gateway |
|---|---:|---|---|
| Server VLAN | 10 | 172.16.10.0/24 | 172.16.10.1 |
| Workstation VLAN | 20 | 172.16.20.0/24 | 172.16.20.1 |
| IT/Admin VLAN | 30 | 172.16.30.0/24 | 172.16.30.1 |
| Lab Management VLAN | 99 | 172.16.99.0/24 | 172.16.99.1 |

## PolkTech Lab VM Static IPs

| VM | IP | VLAN |
|---|---|---|
| PT-OPNSENSE-01 LAN (VLAN 10 gateway) | 172.16.10.1 | 10 |
| PT-OPNSENSE-01 LAN (VLAN 20 gateway) | 172.16.20.1 | 20 |
| PT-DC-01 | 172.16.10.10 | 10 |
| PT-WIN11-01 | DHCP | 20 |