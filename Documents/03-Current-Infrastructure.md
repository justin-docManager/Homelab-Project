# JHL Infrastructure - Current State

**Documentation Date:** [Today's Date]

## Hyper-V Hosts Summary

| Host | RAM | Storage | Virtual Switch | VMs | Default VM Path | Default VHD Path |
|------|-----|---------|----------------|-----|-----------------|------------------|
| JHL-HV-01 | 16GB | 500GB HDD | Sophos WAN (External)<br>Sophos LAN (External) | Sophos XGS (Running) | V:\VM Configs\ | V:\VHDs\ |
| JHL-HV-02 | 24GB | 256GB NVMe | VM-LAN (External) | None | C:\VM Configs\ | C:\VHDs\ |
| JHL-HV-03 | 24GB | 512GB SSD | VM-VLAN (External) | None | C:\VM Configs\ | C:\VHDs\ |

## Observations

### Virtual Switch Configuration
- **JHL-HV-01:** Has two external switches for Sophos firewall (WAN/LAN separation)
- **JHL-HV-02:** Single external switch named "VM-LAN"
- **JHL-HV-03:** Single external switch named "VM-VLAN"
- **Inconsistency:** Different naming conventions across hosts (not following JHL standard yet)

### Virtual Machine Storage
- **JHL-HV-01:** Uses V:\ drive for VM storage (likely the 500GB HDD)
- **JHL-HV-02 & HV-03:** Use C:\ drive for VM storage
- **Note:** We'll standardize storage paths to follow best practices

### Current VMs
- Only **Sophos XGS** is running on JHL-HV-01 (firewall/router)
- JHL-HV-02 and JHL-HV-03 are clean slates ready for lab VMs

## Next Steps

1. **Standardize Virtual Switches** - Create consistent vSwitch naming across all hosts following JHL conventions
2. **Standardize Storage Paths** - Establish consistent VM/VHD storage locations
3. **Network Design** - Design VLAN structure and plan virtual switch configuration
4. **Begin Lab Deployment** - Start building domain infrastructure

## Notes

- JHL-HV-01 is dedicated to Sophos XGS firewall - this will remain and provide routing/firewall for the lab
- JHL-HV-02 has fastest storage (NVMe) - good candidate for high-performance VMs like Domain Controllers
- JHL-HV-03 has most storage capacity (512GB SSD) - good for file servers or multiple VMs
