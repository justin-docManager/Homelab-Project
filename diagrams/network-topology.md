# Network Topology

## Current Network Layout
```
Internet
    |
    | (WAN)
    |
[Sophos XGS Firewall]
192.168.10.1
    |
    | (LAN - VLAN 10)
    |
[HP ProCurve 1810G-24]
192.168.10.2
    |
    |----[JHL-HV-01] 192.168.10.11 (Hosts Sophos VM)
    |----[JHL-HV-02] 192.168.10.12
    |----[JHL-HV-03] 192.168.10.13
    |----[JHL-HV-04] 192.168.10.14
    |----[JHL-HV-05] 192.168.10.15
    |----[JHL-HV-06] 192.168.10.16
    |----[JHL-HV-07] 192.168.10.17
    |----[JHL-HV-08] 192.168.10.18
    |
    |----[Personal PC] (mRemoteNG management)
```

## Network Details

**Management Network**: 192.168.10.0/24  
**VLAN**: 10 (Untagged on ports 1-8, 24)  
**Gateway**: 192.168.10.1 (Sophos)  
**DNS**: 75.75.75.75, 75.75.76.76, 8.8.8.8

## Planned Additions

Future phases will add:
- Domain Controllers (VMs)
- Additional VLANs for network segmentation
- Server VLAN (192.168.20.0/24)
- Workstation VLAN (192.168.30.0/24)

---

*Last Updated: January 10, 2026*
