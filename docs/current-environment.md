# Current Environment Baseline

**Date Documented**: January 10, 2026  
**Status**: Pre-Domain Deployment

## Purpose
This document captures the starting state of the homelab before implementing Active Directory and enterprise services. This serves as a baseline for tracking changes and growth.

---

## Network Configuration

### Lab Network
- **Network**: 192.168.10.0/24
- **Gateway**: 192.168.10.1 (Sophos XG Firewall)
- **DNS**: 75.75.75.75 (Home ISP DNS), 75.75.76.76 (Home ISP DNS), 8.8.8.8 (Google DNS)
- **DHCP**: Currently no DHCP server, the HVs are all set to static IPs.

### Management VLAN
- **VLAN ID**: 10
- **Purpose**: Hyper-V host management
- **Switch Ports**: 
  - Port 24: Untagged (Sophos LAN interface)
  - Ports 1-8: Untagged (Hyper-V host connections)

### Home Network Separation
- **Home Network**: 10.0.0.0/24
- **Isolation Method**: Sophos firewall with separate interfaces
- **Reason**: Keep lab traffic separate from home network

---

## Physical Infrastructure

### Hyper-V Hosts

| Host Name | IP Address | CPU | RAM | Primary Storage | Role |
|-----------|------------|-----|-----|-----------------|------|
| JHL-HV-01 | 192.168.10.11 | i5-6500 | 16GB | 500GB SATA HDD | Sophos Firewall Host |
| JHL-HV-02 | 192.168.10.12 | i5-6500 | 24GB | 256GB NVMe SSD | Available |
| JHL-HV-03 | 192.168.10.13 | i5-6500 | 24GB | 512GB SATA SSD | Available |
| JHL-HV-04 | 192.168.10.14 | i5-6500 | 24GB | 256GB SATA SSD | Available |
| JHL-HV-05 | 192.168.10.15 | i5-6500 | 16GB | 2TB SATA HDD | Available |
| JHL-HV-06 | 192.168.10.16 | i5-6500 | 16GB | 512GB SATA SSD | Available |
| JHL-HV-07 | 192.168.10.17 | i5-6500 | 16GB | 256GB SATA SSD | Available |
| JHL-HV-08 | 192.168.10.18 | i5-6500 | 16GB | 1TB SATA SSD | Available |

### Network Equipment
- **Switch**: HP ProCurve 1810G-24 (192.168.10.2)
  - 24-port managed switch
  - VLAN capable
  - Currently running single VLAN (VLAN 10)

---

## Current Virtual Machines

### JHL-HV-01
- **VM Name**: Sophos XGS
- **Purpose**: Sophos XGS Firewall
- **vCPU**: 4 Cores
- **RAM**: 12GB
- **Network**: Dual NIC (WAN to ISP, LAN to lab network)

---

## Current State Summary

### What's Working
- All 8 Hyper-V hosts online and accessible
- Sophos firewall providing internet access to lab
- Network isolation between lab (192.168.10.0/24) and home (10.0.0.0/24)
- Management access via mRemoteNG from personal PC
- All hosts in WORKGROUP mode (not domain-joined)

### What's Not Configured Yet
- No Active Directory domain
- No centralized DNS (hosts likely using public DNS)
- No centralized DHCP for future VMs
- No file shares or central storage
- No backup solution
- Hyper-V hosts not clustered
- Only one VLAN configured (no network segmentation beyond management)

### Known Limitations
- JHL-HV-01 is dual-purpose (Hyper-V host + Sophos VM) - may impact performance
- Mix of storage types (HDD vs SSD) - need to consider VM placement for performance
- 16GB hosts may be limited for running multiple VMs

---

## Access Methods

### Management Access
- **Method**: RDP via mRemoteNG
- **Source**: Personal PC (assumed on 192.168.10.0/24 network)
- **Credentials**: Local administrator accounts on each host

### Firewall Management
- **Method**: HTTPS to Sophos web interface
- **URL**: Https://192.168.10.1:4444

---

## Next Steps
1. Implement naming conventions standard
2. Deploy first domain controller (DC01)
3. Configure DNS infrastructure
4. Set up DHCP for future VM deployments
5. Join Hyper-V hosts to domain
