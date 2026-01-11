# Naming Conventions Standard

**Version**: 1.0  
**Last Updated**: January 10, 2026  
**Domain**: homelab.local

## Purpose
This document defines the naming standards used throughout the homelab infrastructure. Consistent naming makes it easier to identify systems, troubleshoot issues, and scale the environment.

---

## Domain Information

### Active Directory Domain
- **Domain Name**: homelab.local
- **NetBIOS Name**: HOMELAB
- **Domain Functional Level**: Windows Server 2022 (when implemented)

---

## Naming Structure

### Overall Format
All infrastructure follows the pattern: **JHL-XX-###**

Where:
- **JHL** = Homelab identifier (my initials for personal lab)
- **XX** = Resource type (two letters)
- **###** = Numeric identifier (three digits, zero-padded)

---

## Specific Naming Examples

### Domain Controllers
- **Primary DC**: JHL-DC-001
- **Secondary DC**: JHL-DC-002
- **Pattern**: Sequential numbering for redundancy

### File Servers
- **General File Server**: JHL-FS-001
- **Archive File Server**: JHL-FS-002
- **Pattern**: Purpose can be noted in documentation, naming stays sequential

### Infrastructure Servers
- **DNS Server** (if standalone): JHL-DNS-001
- **DHCP Server** (if standalone): JHL-DHCP-001
- Note: DNS/DHCP typically run on domain controllers in small environments

---

## Network Naming

### VLANs
- **VLAN 10**: Management (Hyper-V hosts)
- **VLAN 20**: Servers (planned)
- **VLAN 30**: Workstations (planned)
- **VLAN 40**: DMZ/Guest (planned)

### Subnets
- **192.168.10.0/24**: Management network
- **192.168.20.0/24**: Server network (planned)
- **192.168.30.0/24**: Workstation network (planned)
- **192.168.40.0/24**: DMZ/Guest network (planned)

---

## IP Address Allocation

### Management Network (192.168.10.0/24)
- **192.168.10.1**: Gateway (Sophos Firewall)
- **192.168.10.2**: Network switch
- **192.168.10.10**: Reserved for future infrastructure
- **192.168.10.11-18**: Hyper-V hosts (JHL-HV-01 through JHL-HV-08)
- **192.168.10.20-29**: Reserved for management VMs
- **192.168.10.30-99**: DHCP pool for management (if implemented)
- **192.168.10.100-199**: Static assignments for infrastructure VMs
- **192.168.10.200-254**: Available for future use

### Server Network (192.168.20.0/24) - Planned
- **192.168.20.1**: Gateway (Sophos VLAN interface)
- **192.168.20.10-29**: Domain Controllers and core services
- **192.168.20.30-99**: File and application servers
- **192.168.20.100-199**: DHCP pool for server VMs
- **192.168.20.200-254**: Available for future use
