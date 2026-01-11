# Homelab Project - Enterprise Infrastructure

## Overview
This repository documents my homelab environment where I'm building and learning enterprise IT infrastructure concepts. The goal is to create a realistic Active Directory domain with multiple services that mirror what you'd find in a business environment.

**Note:** This GitHub was documented with the help of Claude AI.

## Purpose
I'm a Tier 2 Helpdesk Technician working towards a Junior Systems Administrator or Junior Network Engineer role. This homelab helps me develop hands-on experience with:
- Windows Server Administration
- Active Directory, DNS, and DHCP
- Network segmentation and VLANs
- Virtualization with Hyper-V
- Documentation and best practices

## Current Lab Environment

### Physical Hardware
- **8x Dell OptiPlex Hosts** - Running Windows Server 2022 with Hyper-V
  - Mix of 16GB and 24GB RAM
  - i5-6500 processors
  - Various storage configurations (SSD and HDD)

### Network Infrastructure
- **HP ProCurve 1810G-24** managed switch
- **Sophos XG Firewall** (virtualized) - Lab network gateway
- Lab Network: `192.168.10.0/24`
- Management VLAN: VLAN 10

### Current State
Right now the lab has the basic infrastructure in place - all Hyper-V hosts are online and accessible, with a Sophos firewall providing internet access and network isolation from my home network. The hosts have not been joined to a domain yet as I haven't set one up.

## Planned Architecture
I'm building this out in phases to create:
- Active Directory domain (`homelab.local`)
- Redundant domain controllers
- Centralized DNS and DHCP services
- File servers with DFS
- Remote Desktop Services environment
- Additional VLANs for network segmentation
- Backup and monitoring solutions

## Documentation Structure
- **/docs** - Detailed guides and procedures
- **/configs** - Configuration files and scripts
- **/diagrams** - Network diagrams and architecture drawings
  - **/diagrams/screenshots** - Screenshots of relevant information.

## Tools & Technologies
- **Virtualization**: Hyper-V (Windows Server 2022)
- **Operating Systems**: Windows Server 2022, Windows 10/11
- **Networking**: VLANs, routing, firewall rules
- **Management**: PowerShell, mRemoteNG

## Progress Tracking
- [x] Physical infrastructure setup
- [x] Network isolation configured
- [x] Active Directory deployment
- [ ] DNS/DHCP implementation
- [ ] File services
- [ ] Remote Desktop Services
- [ ] Additional VLANs and segmentation

*Last Updated: 1/11/2026*
