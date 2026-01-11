# Domain Controller Deployment

**Server**: JHL-DC-001  
**Date Deployed**: January 11, 2026  
**Deployed By**: Justin
**Host**: JHL-HV-02

---

## Overview

This page goes over the deployment of the first domain controller for the homelab.local domain. JHL-DC-001 serves as the primary DC and DNS server

---

## Server Specifications

**Virtual Machine Configuration**:
- **vCPU**: 2 cores
- **RAM**: 4GB (Dynamic)
- **Storage**: 60GB (JHL-DC-001_OS.vhdx)
- **Network**: Management VLAN (VLAN 10)
- **Operating System**: Windows Server 2022 Standard

**Network Configuration**:
- **IP Address**: 192.168.10.100
- **Subnet Mask**: 255.255.255.0
- **Default Gateway**: 192.168.10.1
- **DNS**: 127.0.0.1 (self), 8.8.8.8 (backup)

---

## Deployment Process

### Phase 1: VM Creation
1. Deployed from golden image (WS2022-Golden.vhdx)
2. Configured with 2 vCPU and 4GB RAM
3. Set to auto-start with Hyper-V host

### Phase 2: Base Configuration
1. Set computer name to JHL-DC-001
2. Configured static IP: 192.168.10.100
3. Set DNS to point to localhost (127.0.0.1)
4. Verified network connectivity

### Phase 3: Active Directory Installation
1. Installed Active Directory Domain Services role via Server Manager
2. Promoted server to domain controller
3. Created new forest: homelab.local
4. NetBIOS name: HOMELAB
5. Installed DNS server role automatically during promotion

---

## Domain Configuration

**Domain Details**:
- **Domain Name**: homelab.local
- **NetBIOS Name**: HOMELAB
- **Forest Functional Level**: Windows Server 2016
- **Domain Functional Level**: Windows Server 2016

**Domain Controller Roles**:
- Primary Domain Controller (PDC)
- Global Catalog (GC)
- DNS Server

---

## DNS Configuration

**DNS Zones Created**:
- Forward Lookup Zone: homelab.local

**DNS Records** (automatically created):
- Domain controller A records
- Service (SRV) records for AD services
- _msdcs subdomain records

---

## Verification Steps Completed

1. Server responds to domain name (homelab.local)
2. Active Directory Users and Computers accessible
3. DNS Manager shows proper zones
4. Domain controller appears in Domain Controllers OU
5. `nltest /dclist:homelab.local` returns JHL-DC-001

---

## Post-Deployment Notes

**What's Working**:
- Domain is operational and responding to queries
- DNS is resolving properly
- Ready to join computers to the domain

**Next Steps**:
- Join Hyper-V hosts to domain
- Create organizational unit (OU) structure
- Set up administrative accounts
- Configure DHCP (future phase)

**Last Updated**: January 11, 2026
