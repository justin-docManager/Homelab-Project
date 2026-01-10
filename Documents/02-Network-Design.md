# JHL Network Design & Architecture

**Documentation Date:** [Today's Date]

## Network Overview

The JHL homelab uses a segmented network design with VLANs to separate traffic types, mimicking enterprise network architecture. The Sophos XGS firewall provides routing between VLANs and connects the lab to the home network.

## Network Topology
```
Internet
   |
Home Router (10.0.0.1)
   |
   | 10.0.0.x/24 (Home Network)
   |
[Sophos XGS on JHL-HV-01]
   | WAN: 10.0.0.x (DHCP from home network)
   | LAN: 192.168.20.1 (Server gateway)
   | Management: 192.168.10.1 (Management gateway)
   |
[HP ProCurve 1810G-24 Switch]
   | Management IP: 192.168.10.2
   | (Trunk ports carrying all VLANs)
   |
   +--- JHL-HV-01 (192.168.10.11)
   +--- JHL-HV-02 (192.168.10.12)
   +--- JHL-HV-03 (192.168.10.13)
   +--- JHL-HV-04 (192.168.10.14)
   +--- JHL-HV-05 (192.168.10.15)
   +--- JHL-HV-06 (192.168.10.16)
   +--- JHL-HV-07 (192.168.10.17)
   +--- JHL-HV-08 (192.168.10.18)
```

---

## VLAN Design

| VLAN ID | Name | Purpose | Subnet | Gateway | DHCP Range |
|---------|------|---------|--------|---------|------------|
| **10** | Management | Infrastructure management (Hyper-V hosts, switch) | 192.168.10.0/24 | 192.168.10.1 | N/A (Static only) |
| **20** | Servers | Domain Controllers, File Servers, Infrastructure VMs | 192.168.20.0/24 | 192.168.20.1 | 192.168.20.100-199 |
| **30** | Clients | Workstations, Test Clients | 192.168.30.0/24 | 192.168.30.1 | 192.168.30.100-199 |
| **40** | DMZ | Internet-facing services (future) | 192.168.40.0/24 | 192.168.40.1 | N/A |
| **50** | Storage | Storage network (iSCSI, SMB) | 192.168.50.0/24 | 192.168.50.1 | N/A |

### VLAN Design Rationale

**VLAN 10 - Management (Already Configured):**
- **Purpose:** Out-of-band management for infrastructure
- Hyper-V host management interfaces (192.168.10.11-18)
- HP ProCurve switch management (192.168.10.2)
- **Security:** Separated from VM traffic, only admins should access
- **Static IPs only:** No DHCP, all management devices have static assignments
- **Best Practice:** Management traffic isolated from production traffic

**VLAN 20 - Servers (Primary Focus):**
- This is where your core infrastructure VMs will live
- Domain Controllers, DNS, DHCP, File Servers
- Most of your lab VMs will be here initially

**VLAN 30 - Clients:**
- Test workstations
- Where you'll test Group Policy, login scripts, etc.
- Simulates user network

**VLAN 40 - DMZ (Future):**
- For internet-facing services
- RD Gateway, Web servers
- Tighter security rules

**VLAN 50 - Storage (Future):**
- Dedicated storage network
- iSCSI traffic, file server replication
- Keeps storage traffic separated

---

## IP Addressing Scheme

### VLAN 10 - Management (192.168.10.0/24) - EXISTING

| IP Address | Hostname | Role | Status |
|------------|----------|------|--------|
| 192.168.10.1 | Sophos-VLAN10-Interface | Default Gateway | Configured |
| 192.168.10.2 | HP-ProCurve-1810G | Switch Management | Configured |
| 192.168.10.11 | JHL-HV-01 | Hyper-V Host | Configured |
| 192.168.10.12 | JHL-HV-02 | Hyper-V Host | Configured |
| 192.168.10.13 | JHL-HV-03 | Hyper-V Host | Configured |
| 192.168.10.14 | JHL-HV-04 | Hyper-V Host | Configured |
| 192.168.10.15 | JHL-HV-05 | Hyper-V Host | Configured |
| 192.168.10.16 | JHL-HV-06 | Hyper-V Host | Configured |
| 192.168.10.17 | JHL-HV-07 | Hyper-V Host | Configured |
| 192.168.10.18 | JHL-HV-08 | Hyper-V Host | Configured |
| 192.168.10.50 | Reserved | Future management VM | Available |
| 192.168.10.51 | Reserved | Future monitoring tools | Available |

**Management VLAN Notes:**
- All IPs are statically assigned
- No DHCP server on this VLAN (security best practice)
- Used for: RDP to hosts, Hyper-V Manager connections, switch management web UI
- Firewall rules should restrict access to admin workstations only

---

### VLAN 20 - Servers (192.168.20.0/24)

| IP Address | Hostname | Role | Notes |
|------------|----------|------|-------|
| 192.168.20.1 | Sophos-VLAN20-Interface | Default Gateway | To be configured |
| 192.168.20.10 | JHL-DC-01 | Primary Domain Controller | DNS, DHCP (Phase 2) |
| 192.168.20.11 | JHL-DC-02 | Secondary Domain Controller | DNS, DHCP (Phase 4) |
| 192.168.20.20 | JHL-FS-01 | File Server | File shares, DFS (Phase 4) |
| 192.168.20.30 | JHL-RDGW-01 | RD Gateway | Remote access (Phase 5) |
| 192.168.20.31 | JHL-RDCB-01 | RD Connection Broker | RDS infrastructure (Phase 5) |
| 192.168.20.32 | JHL-RDSH-01 | RD Session Host | Terminal server (Phase 5) |
| 192.168.20.33 | JHL-RDSH-02 | RD Session Host 2 | Terminal server (Phase 5) |
| 192.168.20.40 | JHL-BACKUP-01 | Backup Server | Backups (Phase 7) |
| 192.168.20.50 | JHL-UTIL-01 | Utility Server | Management tools (Future) |
| 192.168.20.60 | JHL-PRINT-01 | Print Server | Print management (Phase 4) |
| 192.168.20.70 | JHL-WSUS-01 | WSUS Server | Patch management (Phase 7) |
| 192.168.20.100-199 | - | DHCP Pool | Dynamic assignment if needed |

---

### VLAN 30 - Clients (192.168.30.0/24)

| IP Address | Hostname | Role | Notes |
|------------|----------|------|-------|
| 192.168.30.1 | Sophos-VLAN30-Interface | Default Gateway | To be configured |
| 192.168.30.100-199 | - | DHCP Pool | Workstations and test clients |
| 192.168.30.200-220 | - | Reserved | Virtual desktops (VDI) |

---

### VLAN 40 - DMZ (192.168.40.0/24) - Reserved for future use

| IP Address | Assignment | Notes |
|------------|------------|-------|
| 192.168.40.1 | Gateway | To be configured |
| 192.168.40.10-50 | DMZ Servers | Public-facing services |

---

### VLAN 50 - Storage (192.168.50.0/24) - Reserved for future use

| IP Address | Assignment | Notes |
|------------|------------|-------|
| 192.168.50.1 | Gateway | To be configured |
| 192.168.50.10-50 | Storage IPs | iSCSI targets, file server storage NICs |

---

## Hyper-V Virtual Switch Design

Each Hyper-V host will have standardized virtual switches to connect VMs to the appropriate VLANs.

### JHL-HV-01 (Existing Configuration - No Changes)

| Virtual Switch Name | Type | Purpose |
|---------------------|------|---------|
| Sophos WAN | External | Sophos firewall WAN interface |
| Sophos LAN | External | Sophos firewall LAN interface |

**Note:** JHL-HV-01 is dedicated to Sophos XGS - we will not modify its virtual switches.

---

### Standard Virtual Switch Configuration (HV-02 and HV-03)

| Virtual Switch Name | Type | VLAN Config | Purpose |
|---------------------|------|-------------|---------|
| vSwitch-External | External | Trunk (All VLANs) | Connected to physical NIC, carries all VLAN traffic |

**Simplified Approach:**
- Single external switch per host (trunk)
- VLAN assignment happens at VM network adapter level
- No internal switches needed (cleaner, more flexible)

### How VMs Connect to VLANs

When creating a VM:
1. Connect VM to `vSwitch-External`
2. Configure VLAN ID on the VM's network adapter settings
   - Server VMs: VLAN 20
   - Client VMs: VLAN 30
   - Management VMs (if needed): VLAN 10

**Example:**
```powershell
# After creating VM network adapter
Set-VMNetworkAdapterVlan -VMName "JHL-DC-01" -Access -VlanId 20
```

---

## Physical Switch Configuration

### HP ProCurve 1810G-24 Current Configuration

**Management:**
- Management IP: 192.168.10.2/24
- Management VLAN: 10
- Web UI: http://192.168.10.2

### Port Assignments

| Port(s) | Connection | Current Config | Planned Config | Notes |
|---------|------------|----------------|----------------|-------|
| 1 | JHL-HV-01 | Untagged VLAN 10 | **No change** | Sophos XGS host - management only |
| 2 | JHL-HV-02 | Untagged VLAN 10 | **Trunk:** Untagged VLAN 10, Tagged 20,30,40,50 | VM host needs all VLANs |
| 3 | JHL-HV-03 | Untagged VLAN 10 | **Trunk:** Untagged VLAN 10, Tagged 20,30,40,50 | VM host needs all VLANs |
| 4 | JHL-HV-04 | Untagged VLAN 10 | **Trunk:** Untagged VLAN 10, Tagged 20,30,40,50 | Reserved |
| 5 | JHL-HV-05 | Untagged VLAN 10 | **Trunk:** Untagged VLAN 10, Tagged 20,30,40,50 | Reserved |
| 6 | JHL-HV-06 | Untagged VLAN 10 | **Trunk:** Untagged VLAN 10, Tagged 20,30,40,50 | Reserved |
| 7 | JHL-HV-07 | Untagged VLAN 10 | **Trunk:** Untagged VLAN 10, Tagged 20,30,40,50 | Reserved |
| 8 | JHL-HV-08 | Untagged VLAN 10 | **Trunk:** Untagged VLAN 10, Tagged 20,30,40,50 | Reserved |
| 9-23 | Available | Default | Access (TBD) | Available for physical devices |
| **24** | **Sophos XGS LAN** | **Untagged VLAN 10** | **Trunk:** Untagged VLAN 10, Tagged 20,30,40,50 | **Critical: All VLANs for routing** |

### Configuration Notes

**Port 24 (Sophos LAN) - Most Important:**
- This is where all inter-VLAN routing happens
- Must be configured as trunk carrying ALL VLANs
- Sophos will have virtual interfaces for each VLAN on this physical connection
- Native/Untagged VLAN: 10 (Management)
- Tagged VLANs: 20, 30, 40, 50

**Ports 1-8 (Hyper-V Hosts):**
- Port 1 (JHL-HV-01): Access port only - dedicated Sophos host
- Ports 2-8: Will be configured as trunks for VM traffic
- Management traffic (untagged) goes on VLAN 10
- VM traffic (tagged) goes on VLANs 20, 30, 40, 50

**Why This Matters:**
- Sophos needs Port 24 to have all VLANs to route between them
- VM hosts need trunk ports to assign VMs to different VLANs
- Physical separation: Port 1 is just for Sophos host management

## Traffic Flow Examples

### Example 1: Administrator RDP to Hyper-V Host
```
Admin Workstation (10.0.0.x, Home Network)
   ↓
Sophos Routing
   ↓
JHL-HV-02 (192.168.10.12, VLAN 10 Management)
```

### Example 2: VM on Server VLAN accessing internet
```
JHL-DC-01 (192.168.20.10, VLAN 20)
   ↓
vSwitch-External on JHL-HV-02 (VLAN 20 tagged)
   ↓
HP ProCurve Switch (trunk port)
   ↓
Sophos XGS VLAN 20 Interface (192.168.20.1)
   ↓
Sophos routing decision
   ↓
Sophos WAN Interface (10.0.0.x)
   ↓
Home Router → Internet
```

### Example 3: Workstation on Client VLAN accessing File Server
```
Workstation (192.168.30.100, VLAN 30)
   ↓
Default gateway (192.168.30.1 on Sophos)
   ↓
Sophos Inter-VLAN Routing (firewall rules applied)
   ↓
JHL-FS-01 (192.168.20.20, VLAN 20)
```

### Example 4: Domain Controller DNS query from Client
```
Client (192.168.30.100, VLAN 30)
   ↓
DNS query to 192.168.20.10
   ↓
Sophos Inter-VLAN Routing
   ↓
JHL-DC-01 (192.168.20.10, VLAN 20)
   ↓
DNS response back through Sophos
```

---

## Security Considerations

### Management VLAN (VLAN 10) Security

**Firewall Rules (Sophos):**
- Allow inbound to VLAN 10 only from:
  - Home network admin workstation (10.0.0.x)
  - VMs on Server VLAN (for management tools)
- Block all traffic from Client VLAN to Management VLAN
- Log all access attempts to management VLAN

**Best Practices:**
- Never place user workstations on management VLAN
- Use jump/bastion server if remote management needed
- Consider separate admin accounts for management access
- Enable logging on switch for management interface access

---

### Inter-VLAN Firewall Rules (To be implemented on Sophos)

**From VLAN 30 (Clients) → VLAN 20 (Servers):**
- Allow DNS (TCP/UDP 53) to Domain Controllers
- Allow LDAP/Kerberos (TCP/UDP 88, 389, 636) to Domain Controllers
- Allow SMB (TCP 445) to File Servers
- Allow RDP (TCP 3389) to RD Session Hosts
- Allow HTTP/HTTPS to internal web servers
- Deny all other traffic (implicit deny)

**From VLAN 20 (Servers) → VLAN 30 (Clients):**
- Allow established/related connections (return traffic)
- Allow server-initiated connections (updates, monitoring)
- Log unusual outbound connections

**From VLAN 10 (Management) → All VLANs:**
- Allow RDP, WinRM, SSH for administrative access
- Allow monitoring protocols (SNMP, WMI)
- Log all administrative connections

**All VLANs → Internet:**
- Allow HTTP/HTTPS for updates
- Allow DNS to external resolvers
- Block all other outbound by default (optional)

---

## Phase 1 Network Implementation Tasks

**Completed:**
- Network design documented
- VLAN structure defined (including existing VLAN 10)
- IP addressing scheme planned
- Current management network documented

**Next Steps (Task 5):**
1. Document current HP ProCurve VLAN configuration
2. Verify Sophos has interfaces for all VLANs
3. Create standardized virtual switches on HV-02 and HV-03
4. Test VLAN connectivity
5. Verify inter-VLAN routing through Sophos

---

## Network Diagram

[We'll create a visual diagram in the next step]

---

## Current Network State

### What's Already Working
- VLAN 10 (Management) - Fully configured
  - All Hyper-V hosts have static IPs (192.168.10.11-18)
  - HP ProCurve switch accessible at 192.168.10.2
  - Sophos gateway at 192.168.10.1
- Physical connectivity between hosts and switch
- Sophos XGS running on JHL-HV-01

### What Needs Configuration
- VLANs 20, 30, 40, 50 on HP ProCurve
- Sophos interfaces for VLANs 20, 30, 40, 50
- Virtual switches on HV-02 and HV-03
- Inter-VLAN routing rules on Sophos
- Firewall rules on Sophos

---

## Notes and Best Practices

### Why Separate Management VLAN?

1. **Security:** Management access isolated from production traffic
2. **Availability:** If production VLANs have issues, you can still manage infrastructure
3. **Performance:** Management traffic doesn't compete with VM traffic
4. **Compliance:** Many frameworks require separate management networks
5. **Troubleshooting:** Easier to diagnose issues when management is isolated

### Design Decisions

- **VLAN 10 for Management:** Industry standard, low number for infrastructure
- **VLAN 20 for Servers:** Keeps servers separate from management
- **Static IPs on Management VLAN:** No DHCP = no dependency failures
- **Trunk ports to VM hosts:** Flexibility to assign VMs to any VLAN
- **Single vSwitch per host:** Simpler than multiple internal switches

### Enterprise Alignment

This design mirrors enterprise practices:
- Separate management plane from data plane
- VLAN segmentation by function
- Centralized routing through firewall
- Static IP allocation for infrastructure
- Trunk/access port strategy on physical switches
