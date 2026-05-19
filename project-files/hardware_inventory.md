# Hardware Inventory

## Proxmox Host

| Component | Details |
|---|---|
| Model | Dell OptiPlex 3080 SFF |
| CPU | Intel Core i5-10500 SRH3A 3.10GHz |
| RAM | Samsung 8GB 1Rx8 PC4-2666V-UA2-11 + SKhynix 8GB 1Rx8 PC4-2666V-UA2-11 = 16GB total |
| Storage | Western Digital PC SN520 NVMe 256GB (dev/nvme0n1) |
| Onboard NIC | Realtek RTL8111 1G (nic0) - used for Proxmox management |
| PCIe NIC | Intel 82576 Gigabit dual-port PCIe X1 (nic1, nic2) - used for OPNsense WAN and spare |
| Power Supply | Dell 200W (H200EBS-01) |
| Virtualization Platform | Proxmox VE 9.1.1 |

## Network Equipment

| Device | Purpose |
|---|---|
| Xfinity Gateway | Internet gateway and home router |
| 5-Port Unmanaged Switch | Extends home LAN |
| Dell OptiPlex 3080 | Proxmox host |
| User/Host PC | Admin workstation |

## Notes
The unmanaged switch does not support VLAN tagging or VLAN separation. Lab isolation is handled inside Proxmox and OPNsense. The Realtek RTL8111 is the onboard motherboard NIC. The Intel 82576 is a dual-port PCIe add-in card installed in the PCIe X1 slot.