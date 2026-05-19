# VM Inventory

| VM Name | Role | VLAN | IP Address | OS | Notes |
|---|---|---|---|---|---|
| PT-OPNSENSE-01 | Firewall/Router | WAN + LAN/VLANs | 10.0.0.x / 172.16.x.1 | OPNsense | Routes lab networks |
| PT-DC-01 | Domain Controller | VLAN 10 | 172.16.10.10 | Windows Server | AD DS, DNS, DHCP optional |
| PT-WIN11-01 | Workstation | VLAN 20 | DHCP | Windows 11 | Domain-joined client |