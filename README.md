# Enterprise Virtualization Home Lab

## Project Overview
This lab simulates a medium-sized enterprise environment using a "Virtualization First" approach. 

### Infrastructure Components
* **Hypervisors:** 8x Windows Server 2022 Hyper-V Hosts (Old Optiplex 7040d)
* **Networking:** Sophos XGS (VM) & HP ProCurve 1810G-24
* **Identity:** Active Directory DS (Virtualized)
* **IP Scheme:** 192.168.0.0/16 (Separated from Home 10.0.0.0/8)

### Project Objectives
* **Network Engineering:** Implementation of VLANs, Inter-VLAN routing, and Firewall using Sophos XGS.
* **Systems Administration:** Deployment of a Windows Server 2022 environment including Active Directory Domain Services (ADDS).
* **Core Services:** High-availability configuration of DNS and DHCP.
* **Storage & Continuity:** Implementation of Network Attached Storage (NAS) roles and automated backup solutions.
* **Virtualization:** Management of a multi-node Hyper-V cluster environment.

### Standardization & Governance
To ensure scalability and ease of management, the following naming conventions are applied:

| Asset Type | Convention | Example |
| :--- | :--- | :--- |
| Physical Hosts | `JHL-HV-[01-08]` | `JHL-HV-01` |
| Virtual Servers | `JHL-[Role]-[01-99]` | `JHL-DC-01` |
| Client VMs | `JHL-[OS]-[01-99]` | `JHL-WIN11-01` |
| Virtual Switches | `vSwitch-[VLAN ]` | `vSwitch-MGMT` |

### Project Roadmap
* [ ] **Phase 1: Design & Documentation** (Current) - IPAM, Asset Registry, and GitHub setup.
* [ ] **Phase 2: Core Networking** - HP ProCurve VLAN configuration and Sophos XGS initialization.
* [ ] **Phase 3: Hypervisor Deployment** - Installation of Windows Server 2022 on all physical nodes.
* [ ] **Phase 4: Identity & Core Services** - Active Directory Forest creation, DNS, and DHCP migration.
* [ ] **Phase 5: Storage & Backups** - File Server role and backup repository setup.
