# IP Address Management (IPAM)

## VLAN 10 - Management (Physical Hosts & Switch)
*Subnet: 192.168.10.0/24 | Gateway: 192.168.10.1*

| IP Address    | Device               |
|:--------------|:---------------------|
| 192.168.10.2  | HP ProCurve Switch   |
| 192.168.10.11 | JHL-HV-01 |
| 192.168.10.12 | JHL-HV-02 |
| 192.168.10.13 | JHL-HV-03 |
| 192.168.10.14 | JHL-HV-04 |
| 192.168.10.15 | JHL-HV-05 |
| 192.168.10.16 | JHL-HV-06 |
| 192.168.10.17 | JHL-HV-06 |
| 192.168.10.18 | LAB-HV-08 |

## VLAN 20 - Servers (Virtual Machines)
*Subnet: 192.168.20.0/24 | Gateway: 192.168.20.1*

| IP Address    | VM Name     | Role                 | Host       |
|:--------------|:------------|:---------------------|:-----------|
| 192.168.20.1  | Sophos-XGS  | Firewall/Gateway     | JHL-HV-08  |
| 192.168.20.11 | JHL-DC-01   | Domain Controller 1  | JHL-HV-02  |
| 192.168.20.12 | JHL-DC-02   | Domain Controller 2  | JHL-HV-03  |
| 192.168.20.20 | JHL-FS-01   | File Server          | JHL-HV-04  |
