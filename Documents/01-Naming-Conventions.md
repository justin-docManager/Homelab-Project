# JHL Naming Conventions

## Overview
This document defines the naming standards used throughout the JHL homelab infrastructure. These conventions follow enterprise best practices to ensure consistency, scalability, and clarity.

## Organization Code
**JHL** - Used as prefix for all infrastructure components

## Domain Structure
- **Active Directory Domain:** `homelab.local`
- **NetBIOS Name:** `HOMELAB`
- **Forest Functional Level:** Windows Server 2016 (supports Server 2022)

---

## Physical Host Naming

**Format:** `JHL-HV-##`

- **JHL** = Organization code
- **HV** = Hyper-V host
- **##** = Sequential number (01, 02, 03...)

### Current Physical Hosts
| Hostname | CPU | RAM | Primary Disk | Role |
|----------|-----|-----|--------------|------|
| JHL-HV-01 | i5-6500 4c | 16GB | 500GB SATA HDD | Hyper-V Host (Sophos Firewall) |
| JHL-HV-02 | i5-6500 4c | 24GB | 256GB NVMe SSD | Hyper-V Host |
| JHL-HV-03 | i5-6500 4c | 24GB | 512GB SATA SSD | Hyper-V Host |
| JHL-HV-04 | i5-6500 4c | 24GB | 256GB SATA SSD | Hyper-V Host (Reserved) |
| JHL-HV-05 | i5-6500 4c | 16GB | 2TB SATA HDD | Hyper-V Host (Reserved) |
| JHL-HV-06 | i5-6500 4c | 16GB | 512GB SATA SSD | Hyper-V Host (Reserved) |
| JHL-HV-07 | i5-6500 4c | 16GB | 256GB SATA SSD | Hyper-V Host (Reserved) |
| JHL-HV-08 | i5-6500 4c | 16GB | 1TB SATA SSD | Hyper-V Host (Reserved) |

---

## Virtual Machine Naming

**Format:** `JHL-[ROLE]-[##]`

### Role Codes
| Code | Description | Example |
|------|-------------|---------|
| DC | Domain Controller | JHL-DC-01 |
| DNS | DNS Server (if separated) | JHL-DNS-01 |
| DHCP | DHCP Server (if separated) | JHL-DHCP-01 |
| FS | File Server | JHL-FS-01 |
| PRINT | Print Server | JHL-PRINT-01 |
| RDS | Remote Desktop Services | JHL-RDS-01 |
| RDGW | RD Gateway | JHL-RDGW-01 |
| RDCB | RD Connection Broker | JHL-RDCB-01 |
| RDSH | RD Session Host | JHL-RDSH-01 |
| RDWA | RD Web Access | JHL-RDWA-01 |
| VDI | VDI Connection Broker | JHL-VDI-01 |
| VDIH | VDI Host | JHL-VDIH-01 |
| WSUS | Windows Update Services | JHL-WSUS-01 |
| BACKUP | Backup Server | JHL-BACKUP-01 |
| PKI | Certificate Authority | JHL-PKI-01 |
| SQL | SQL Server | JHL-SQL-01 |
| WEB | Web Server (IIS) | JHL-WEB-01 |
| APP | Application Server | JHL-APP-01 |
| MON | Monitoring Server | JHL-MON-01 |
| LOG | Log Collection Server | JHL-LOG-01 |
| UTIL | Utility/Management Server | JHL-UTIL-01 |
| JUMP | Jump/Bastion Server | JHL-JUMP-01 |
| NPS | Network Policy Server (RADIUS) | JHL-NPS-01 |

### Examples
- Primary Domain Controller: `JHL-DC-01`
- Secondary Domain Controller: `JHL-DC-02`
- File Server: `JHL-FS-01`
- RD Session Host 1: `JHL-RDSH-01`
- RD Session Host 2: `JHL-RDSH-02`

---

## Network Infrastructure Naming

### VLANs
**Format:** `VLAN[ID] - [Description]`

| VLAN ID | Name | Purpose | Subnet |
|---------|------|---------|--------|
| 10 | VLAN10 - Management | Infrastructure management | 192.168.10.0/24 |
| 20 | VLAN20 - Servers | Server infrastructure | 192.168.20.0/24 |
| 30 | VLAN30 - Clients | Client workstations | 192.168.30.0/24 |
| 40 | VLAN40 - DMZ | Internet-facing services | 192.168.40.0/24 |
| 50 | VLAN50 - Storage | Storage network (iSCSI/SMB) | 192.168.50.0/24 |
| 99 | VLAN99 - Guest | Guest/isolated network | 192.168.99.0/24 |

### Hyper-V Virtual Switches
**Format:** `vSwitch-[Purpose]`

- `vSwitch-External` - External network connectivity
- `vSwitch-Internal-Management` - Management network (VLAN 10)
- `vSwitch-Internal-Servers` - Server network (VLAN 20)
- `vSwitch-Internal-Clients` - Client network (VLAN 30)
- `vSwitch-Private-Storage` - Storage network (VLAN 50)

---

## IP Address Scheme

### Subnet Structure
**Format:** `192.168.[VLAN].0/24`

### Static IP Allocation Within Subnets
- `.1` - Gateway/Router
- `.2-.9` - Network infrastructure (switches, firewalls, hypervisors)
- `.10-.99` - Servers (static assignments)
- `.100-.199` - DHCP pool for workstations/devices
- `.200-.249` - Printers and IoT devices
- `.250-.254` - Reserved for future use

### Example: VLAN 20 (Servers - 192.168.20.0/24)
| IP Address | Assignment |
|------------|------------|
| 192.168.20.1 | Default Gateway (Sophos/Router) |
| 192.168.20.10 | JHL-DC-01 (Primary DC) |
| 192.168.20.11 | JHL-DC-02 (Secondary DC) |
| 192.168.20.20 | JHL-FS-01 (File Server) |
| 192.168.20.30 | JHL-RDS-01 |
| 192.168.20.40 | JHL-BACKUP-01 |
| 192.168.20.100-.199 | DHCP Pool (if needed) |

### Example: VLAN 10 (Management - 192.168.10.0/24)
| IP Address | Assignment |
|------------|------------|
| 192.168.10.1 | Default Gateway |
| 192.168.10.2 | HP ProCurve Switch Management |
| 192.168.10.3 | Sophos XGS Management |
| 192.168.10.5 | JHL-HV-01 Management Interface |
| 192.168.10.6 | JHL-HV-02 Management Interface |
| 192.168.10.7 | JHL-HV-03 Management Interface |
| 192.168.10.50 | JHL-UTIL-01 (Management/Monitoring) |

### Example: VLAN 30 (Clients - 192.168.30.0/24)
| IP Address | Assignment |
|------------|------------|
| 192.168.30.1 | Default Gateway |
| 192.168.30.100-.199 | DHCP Pool for workstations |

---

## Active Directory Structure

### Organizational Units (OU)
**Top-Level OUs:**
```
homelab.local
├── JHL-Domain-Controllers
├── JHL-Computers
│   ├── Servers
│   ├── Workstations
│   ├── Laptops
│   └── Virtual Desktops
├── JHL-Users
│   ├── Administrators
│   ├── IT-Staff
│   ├── Standard-Users
│   └── Service-Accounts
├── JHL-Groups
│   ├── Security-Groups
│   └── Distribution-Groups
└── JHL-Resources
    ├── Printers
    └── Shared-Resources
```

### User Account Naming
**Format:** `firstname.lastname`
- Example: `john.smith`
- Admin accounts: `firstname.lastname-admin` (e.g., `john.smith-admin`)
- Service accounts: `svc-[service]` (e.g., `svc-backup`, `svc-sql`, `svc-wsus`)

### Group Naming
**Format:** `[Type]-[Purpose]-[Permission]`
- Security groups: `SG-[Resource]-[Permission]`
  - Example: `SG-FileShare-Finance-RW` (Read/Write)
  - Example: `SG-FileShare-Finance-RO` (Read-Only)
  - Example: `SG-RDS-Users`
- Distribution groups: `DG-[Department]`
  - Example: `DG-IT-Department`
  - Example: `DG-All-Users`

---

## Storage Naming

### File Shares
**Format:** `[Department/Purpose]$` (hidden) or `[Department/Purpose]` (visible)

Examples:
- `IT$` - IT Department share (hidden)
- `Finance$` - Finance Department share (hidden)
- `UserProfiles$` - Roaming profiles (hidden)
- `Software$` - Software repository (hidden)
- `Shares` - General shared folders (visible)
- `Public` - Public read-only resources (visible)

### DFS Namespaces
**Format:** `\\homelab.local\[namespace]`

Examples:
- `\\homelab.local\shares` - DFS root for department shares
- `\\homelab.local\data` - DFS root for data shares
- `\\homelab.local\software` - Software distribution

### Virtual Hard Disks (VHDX)
**Format:** `[VMName]-[Purpose].vhdx`

Examples:
- `JHL-DC-01-OS.vhdx` - Operating system disk
- `JHL-DC-01-Data.vhdx` - Data disk
- `JHL-FS-01-OS.vhdx`
- `JHL-FS-01-Storage.vhdx`

---

## Group Policy Objects (GPO)

**Format:** `[Scope]-[Purpose]-[Version]`

Examples:
- `Domain-Password-Policy-v1`
- `Domain-Account-Lockout-Policy-v1`
- `Servers-Security-Baseline-v1`
- `Servers-Windows-Update-Config-v1`
- `Workstations-Desktop-Config-v1`
- `Workstations-Drive-Mappings-v1`
- `Users-Folder-Redirection-v1`
- `Users-Login-Script-v1`

---

## Backup Naming

### Backup Jobs
**Format:** `[Target]-[Type]-[Frequency]`

Examples:
- `DC-Full-Daily`
- `FS-Incremental-Hourly`
- `SQL-Transaction-Log-15min`
- `AllVMs-Full-Weekly`

### Backup Storage Locations
**Format:** `\\[BackupServer]\Backups\[Year]\[Month]\[Target]`

Example:
- `\\JHL-BACKUP-01\Backups\2024\01\JHL-DC-01`

---

## Documentation Standards

### Document Naming
**Format:** `[##-Category-Name.md]`

Examples:
- `00-Lab-Architecture.md`
- `01-Naming-Conventions.md`
- `02-Network-Design.md`
- `10-Active-Directory-Setup.md`
- `11-DNS-Configuration.md`
- `12-DHCP-Configuration.md`

### Diagram Naming
**Format:** `[Description]-[Type]-[Date].png`

Examples:
- `JHL-Network-Topology-2024-01.png`
- `JHL-AD-Structure-Diagram-2024-01.png`
- `JHL-VLAN-Design-2024-01.png`

### Screenshot Naming
**Format:** `[Step]-[Description].png`

Examples:
- `01-Hyper-V-Virtual-Switch-Creation.png`
- `02-DC-Static-IP-Configuration.png`

---

## Notes and Best Practices

### Why These Conventions Matter
1. **Consistency:** Makes troubleshooting easier when everything follows a pattern
2. **Scalability:** Easy to add new resources following established patterns
3. **Documentation:** Clear naming makes documentation self-explanatory
4. **Professionalism:** Shows hiring managers you understand enterprise standards
5. **Automation:** Predictable names make scripting and automation simpler

### Naming Tips
- Always use uppercase for organization code (JHL)
- Use hyphens (-) to separate components, not underscores
- Keep names concise but descriptive
- Avoid special characters except hyphens
- Use sequential numbering (01, 02, 03...) not (1, 2, 3...)
- Document exceptions when they occur
