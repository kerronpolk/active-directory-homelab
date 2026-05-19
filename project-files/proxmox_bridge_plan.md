# Proxmox Bridge Plan
## PolkTech SOHO Active Directory Homelab

---

## Objective

Define the mapping between physical NIC ports on the Dell OptiPlex 3080 and Proxmox VE Linux bridges before installation begins. This document serves as the reference for Phase 1 network configuration and prevents incorrect bridge-to-NIC assignments during setup.

---

## Environment

| Component | Details |
|---|---|
| Host | Dell OptiPlex 3080 SFF |
| Hypervisor | Proxmox VE 9.1.1 |
| Onboard NIC | Realtek RTL8111 1G (1x RJ45) |
| PCIe NIC | Intel 82576 Gigabit dual-port PCIe X1 (2x RJ45) |
| Total Ports | 3x 1G RJ45 |
| Home LAN | 10.0.0.0/24 |
| Lab Networks | 172.16.10.0/24, 172.16.20.0/24 |

---

## Design Rationale

Proxmox VE uses Linux bridges to connect VMs to networks. A bridge behaves like a virtual switch — physical NICs can be attached to a bridge to give it an uplink to a physical network, or a bridge can be left internal with no physical NIC attached.

This lab uses three types of bridges:

**Management bridge (vmbr0)**
Attached to the onboard Realtek RTL8111 NIC (nic0). Proxmox itself uses this bridge for web UI access and management. It sits on the home LAN (10.0.0.0/24) and has a static IP of 10.0.0.2.

**WAN bridge (vmbr1)**
Attached to Intel 82576 port A (nic1). This bridge has no IP address assigned on the Proxmox side. It exists solely to give the OPNsense VM a path to the home LAN for its WAN interface. OPNsense WAN will receive a DHCP address from the Xfinity Gateway through this bridge.

**Spare bridge (vmbr2)**
Attached to Intel 82576 port B (nic2). Reserved for future use. Not assigned to any VM at this time.

**Internal lab bridges (vmbr10, vmbr20)**
No physical NIC attached. These are fully internal to Proxmox. Lab VMs connect to these bridges and all traffic between them is routed through the OPNsense VM. These bridges have no direct path to the home LAN.

This design ensures that Windows Server and Windows 11 lab VMs are never directly exposed to the home LAN and cannot receive DHCP from the Xfinity Gateway.

---

## Bridge Configuration Table

| Bridge | Physical NIC | VLAN | Subnet | Proxmox IP | Role |
|---|---|---|---|---|---|
| vmbr0 | nic0 (Realtek RTL8111) | None | 10.0.0.0/24 | 10.0.0.2 | Proxmox management |
| vmbr1 | nic1 (Intel 82576 port A) | None | 10.0.0.0/24 | None | OPNsense WAN uplink |
| vmbr2 | nic2 (Intel 82576 port B) | None | None | None | Spare |
| vmbr10 | None (internal) | VLAN 10 | 172.16.10.0/24 | None | Server VLAN |
| vmbr20 | None (internal) | VLAN 20 | 172.16.20.0/24 | None | Workstation VLAN |
---

## VM Network Interface Assignments

| VM | Bridge | Interface Role |
|---|---|---|
| PT-OPNSENSE-01 | vmbr1 | WAN (uplink to home LAN) |
| PT-OPNSENSE-01 | vmbr10 | LAN / VLAN 10 gateway (172.16.10.1) |
| PT-OPNSENSE-01 | vmbr20 | LAN / VLAN 20 gateway (172.16.20.1) |
| PT-DC-01 | vmbr10 | Server VLAN (172.16.10.10 static) |
| PT-WIN11-01 | vmbr20 | Workstation VLAN (DHCP) |

---

## NIC Identification Steps

Proxmox assigns Linux device names to NICs during installation. These names will look like `eno1`, `enp2s0`, or `enp3s0` and may not obviously correspond to which physical port they represent.

After Proxmox is installed, use the following steps to identify which device name belongs to which physical port before configuring bridges.

**Step 1 — Open the Proxmox shell**
Log into the Proxmox web UI at `https://10.0.0.2:8006`. Navigate to your node and open Shell.

**Step 2 — List all network interfaces**
```bash
ip link show
```
Note all interface names that appear. Ignore `lo` (loopback). You should see three interfaces corresponding to your three physical ports.

**Step 3 — Identify each port by cable**
Plug a network cable into one port at a time and run:
```bash
ip link show
```
The interface that changes from `DOWN` to `UP` is the port you just plugged into. Label each port with its Linux device name before proceeding.

**Step 4 — Record your findings**
Update the table below with the actual device names after identification:

| Physical Port | Linux Device Name | Assigned Bridge |
|---|---|---|
| Onboard NIC (Realtek RTL8111) | nic0 | vmbr0 |
| PCIe port A (Intel 82576) | nic1 | vmbr1 |
| PCIe port B (Intel 82576) | nic2 | vmbr2 |

---

## Proxmox Network Configuration

Once NIC names are identified, configure bridges in the Proxmox web UI under:
**Datacenter → [Node name] → Network → Create → Linux Bridge**

**vmbr0** (created automatically during install)
- Bridge ports: onboard NIC device name
- IP address: 10.0.0.2/24
- Gateway: 10.0.0.1
- Comment: Proxmox management - home LAN

**vmbr1** (create manually)
- Bridge ports: PCIe port A device name
- IP address: none
- Gateway: none
- Comment: OPNsense WAN uplink - home LAN

**vmbr2** (create manually)
- Bridge ports: PCIe port B device name
- IP address: none
- Gateway: none
- Comment: Spare - reserved

**vmbr10** (create manually)
- Bridge ports: none
- IP address: none
- Gateway: none
- Comment: Internal - Server VLAN 10 - 172.16.10.0/24

**vmbr20** (create manually)
- Bridge ports: none
- IP address: none
- Gateway: none
- Comment: Internal - Workstation VLAN 20 - 172.16.20.0/24

After creating all bridges, click **Apply Configuration** and verify no errors appear.

---

## Validation

After bridge configuration is complete, verify the following before creating any VMs:

- [ ] Proxmox web UI is accessible at `https://10.0.0.2:8006` from the host PC
- [ ] `ip addr show vmbr0` shows 10.0.0.2/24
- [ ] `ip addr show vmbr1` shows no IP address
- [ ] `ip addr show vmbr10` shows no IP address
- [ ] `ip addr show vmbr20` shows no IP address
- [ ] Proxmox can reach the internet (run `ping 8.8.8.8` from the shell)

---

## Screenshots to Capture

- Proxmox Network tab showing all five bridges listed
- Each bridge's configuration panel showing correct port assignments
- Shell output of `ip link show` with all interfaces visible
- Shell output of `ping 8.8.8.8` confirming internet access from Proxmox

---

## Common Mistakes

**Wrong NIC assigned to vmbr0 during install**
If Proxmox is installed using the PCIe NIC instead of the onboard NIC, vmbr0 will be on the wrong port. You can correct this post-install by editing `/etc/network/interfaces` directly, but it is easier to identify ports first using a live USB before running the installer.

**vmbr1 given an IP address**
vmbr1 should have no IP address on the Proxmox side. It is a pass-through uplink for OPNsense WAN only. Assigning an IP here could cause routing conflicts.

**Internal bridges accidentally given a physical NIC**
vmbr10 and vmbr20 must have no bridge port assigned. If a physical NIC is attached to these bridges, lab VM traffic could reach the home LAN directly, bypassing OPNsense entirely.

---

## Real-World IT Relevance

In enterprise environments, hypervisor network configuration is a foundational skill for virtualization administrators. Mapping physical NICs to virtual switches, separating management traffic from VM traffic, and designing internal-only networks are standard practices in VMware vSphere, Microsoft Hyper-V, and Proxmox VE deployments. This bridge plan mirrors the kind of network design documentation a junior systems administrator or virtualization engineer would be expected to produce before provisioning a new host.

---

## Notes

- vmbr2 is reserved and not currently used. It can be repurposed for a second OPNsense WAN interface, a dedicated management VLAN, or an additional lab segment in a future phase.
- If the Xfinity Gateway DHCP pool conflicts with 10.0.0.2, change the Proxmox management IP to 10.0.0.50 or another address outside the DHCP range before finalizing installation.
- Bridge names (vmbr0, vmbr1, etc.) are convention only. The numbers have no technical significance but should remain consistent across all project documentation.

