# Active Directory Homelab

> A SOHO Active Directory homelab built on Proxmox VE with OPNsense, Windows Server, and Windows 11. Designed to demonstrate practical IT support, systems administration, networking, and virtualization skills in a realistic small business environment.

**Status: In Progress**

---

## About This Project

This project builds a fully functional small office/home office (SOHO) IT environment for a fictional company called **PolkTech**. The lab runs entirely on a single refurbished Dell OptiPlex 3080 using Proxmox VE as the hypervisor.

The goal is to replicate the kind of environment a junior IT support technician, help desk analyst, or systems administrator would encounter in a real small business — including a domain controller, DNS, DHCP, Group Policy, firewall, and domain-joined workstations.

---

## Skills Demonstrated

- Hypervisor setup and virtual machine management (Proxmox VE)
- Network design and segmentation (OPNsense, VLANs, Linux bridges)
- Firewall configuration and routing
- Windows Server administration
- Active Directory Domain Services (AD DS)
- DNS and DHCP configuration
- Group Policy creation and management
- Domain join and workstation management
- Hardware refurbishment and IT asset management
- Technical documentation for a GitHub portfolio

---

## Tech Stack

| Technology | Role |
|---|---|
| Proxmox VE 9.1 | Hypervisor |
| OPNsense | Virtual firewall and router |
| Windows Server 2022 | Domain controller, DNS, DHCP |
| Windows 11 Pro | Domain-joined workstation |
| Dell OptiPlex 3080 | Physical host hardware |
| Intel 82576 dual-port NIC | Network segmentation |

---

## Network Design

The lab uses OPNsense as a virtual firewall to separate the home network from the PolkTech lab environment. Lab VMs never connect directly to the home network and cannot receive DHCP from the home router.

![PolkTech Lab Network Diagram](screenshots/phase-0-pre-build/network_diagram.png)

| Network | VLAN | Subnet | Gateway |
|---|---|---|---|
| Home LAN | — | 10.0.0.0/24 | 10.0.0.1 |
| Server VLAN | 10 | 172.16.10.0/24 | 172.16.10.1 |
| Workstation VLAN | 20 | 172.16.20.0/24 | 172.16.20.1 |

---

## Lab Phases

| Phase | Description | Status |
|---|---|---|
| [Phase 0 — Hardware Acquisition and Pre-Build Setup](labs/phase-0-pre-build/) | Hardware sourcing, refurbishment, and pre-build verification | ✅ Complete |
| [Phase 1 — Proxmox VE Installation and Network Configuration](labs/phase-1-proxmox/) | Proxmox installation, bridge configuration, and validation | ✅ Complete |
| Phase 2 — OPNsense VM Installation and Configuration | Virtual firewall setup, VLAN configuration, and routing | 🔄 In Progress |
| Phase 3 — Windows Server and Active Directory | Domain controller promotion, DNS, DHCP, and AD DS | ⏳ Planned |
| Phase 4 — Windows 11 Client and Domain Join | Workstation configuration and domain join | ⏳ Planned |
| Phase 5 — Group Policy and Validation | GPO creation, DHCP relay, and full lab validation | ⏳ Planned |

---

## Hardware

| Component | Details |
|---|---|
| Host | Dell OptiPlex 3080 SFF |
| CPU | Intel Core i5-10500 3.10GHz |
| RAM | 16GB DDR4 (Samsung 8GB + SKhynix 8GB) |
| Storage | Western Digital PC SN520 NVMe 256GB |
| Onboard NIC | Realtek RTL8111 1G |
| PCIe NIC | Intel 82576 dual-port 1G PCIe X1 |
| PSU | Dell 200W |

---

## Project Files

Supporting documentation and reference files are available in the [`project-files/`](project-files/) directory:

- [`ip_address_plan.md`](project-files/ip_address_plan.md) — IP addressing and VLAN plan
- [`vm_and_naming_reference.md`](project-files/vm_and_naming_reference.md) — VM inventory, naming conventions, and domain standards
- [`proxmox_bridge_plan.md`](project-files/proxmox_bridge_plan.md) — Proxmox NIC to bridge mapping

---

## Disclaimer

This is a personal homelab project built for learning and portfolio purposes. All company names, domain names, and organizational details are fictional. No sensitive personal or network information has been intentionally published.
