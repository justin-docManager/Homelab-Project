# Domain Join Process - Hyper-V Hosts

**Date Completed**: January 11, 2026  
**Domain**: homelab.local  
**Total Hosts Joined**: 8

---

## Overview

This covers the process of joining all Hyper-V hosts to the homelab.local domain.

---

## Prerequisites

Before joining hosts to the domain:
- Domain controller (JHL-DC-001) operational
- DNS configured and resolving homelab.local
- Network connectivity verified between hosts and DC

---

## DNS Configuration

All hosts were configured with the following DNS settings before domain join:

**DNS Servers**:
- Primary: 192.168.10.100 (JHL-DC-001)
- Secondary: 8.8.8.8 (Google DNS as backup)

**Verification Command**:
```
nslookup homelab.local
```
result: 192.168.10.100

---

## Domain Join Process

### Standard Process (Used for most hosts)

1. **Update DNS Settings**:
   - Control Panel → Network Connections
   - Network adapter properties → IPv4 settings
   - Set DNS to 192.168.10.100 (primary), 8.8.8.8 (secondary)

2. **Join Domain**:
   - System Properties → Computer Name → Change
   - Select "Domain" and enter: homelab.local
   - Authenticate with: HOMELAB\Administrator
   - Restart when prompted

3. **Verify Join**:
   - Check Active Directory Users and Computers on DC

---

## Hosts Joined

| Host Name | IP Address | Order | Notes |
|-----------|------------|-------|-------|
| JHL-HV-03 | 192.168.10.13 | 1st | Test case - joined successfully |
| JHL-HV-04 | 192.168.10.14 | 2nd | No issues |
| JHL-HV-05 | 192.168.10.15 | 3rd | No issues |
| JHL-HV-06 | 192.168.10.16 | 4th | No issues |
| JHL-HV-07 | 192.168.10.17 | 5th | No issues |
| JHL-HV-08 | 192.168.10.18 | 6th | No issues |
| JHL-HV-01 | 192.168.10.11 | 7th | Sophos host - required reboot before successful join |
| JHL-HV-02 | 192.168.10.12 | 8th | DC host - brief domain outage during reboot as expected |

---

## Special Cases

### JHL-HV-01 (Sophos Firewall Host)

**Challenge**: Initial domain join failed with "general network error" despite DNS resolving correctly.

**Resolution**:
- Rebooted the host
- Retry successful after reboot
- Likely due to network stack caching or dual NIC configuration

**Notes**:
- This host has dual NICs for Sophos VM (WAN/LAN separation)
- Sophos VM automatically restarted after host reboot
- Internet connectivity restored immediately

### JHL-HV-02 (Domain Controller Host)

**Challenge**: This host runs JHL-DC-001 VM, so joining it causes temporary domain unavailability.

**Process**:
- Verified JHL-DC-001 VM set to auto-start with host
- Joined domain normally
- During reboot: Domain offline for approximately 2-3 minutes
- JHL-DC-001 auto-started successfully
- Verified domain functionality from other hosts

**Impact**: Minimal - brief outage acceptable in lab environment

---

## Post-Join Verification

### On Domain Controller (JHL-DC-001)
- Opened Active Directory Users and Computers
- Verified all 8 hosts appear in Computers OU
- All hosts showing as domain members

---

## Lessons Learned

**What Went Well**:
- DNS configuration was straightforward
- Most hosts joined without issues
- Domain join process is quick (5-10 minutes per host)

**Challenges Encountered**:
- JHL-HV-01 required reboot before successful join
- Brief domain outage when joining HV-02 was expected but still notable
- Network error on HV-01 was resolved by simple reboot

**Best Practices Identified**:
- Always update DNS before attempting domain join
- Verify DNS resolution with nslookup before joining
- Save critical VM hosts (those running DCs, firewalls) for last
- Ensure VMs are set to auto-start before rebooting hosts

---

## Next Steps

With all hosts domain-joined, the following tasks are now possible:

- Create Organizational Unit (OU) structure
- Move host computer objects to appropriate OUs
- Create domain user accounts for management
- Apply Group Policies to hosts
- Configure centralized time synchronization
