# IP Address Management (IPAM)

## VLAN 10 - Management (Physical Infrastructure)
*Subnet: 192.168.10.0/24 | Gateway: 192.168.10.1*

| IP Address    | Device               | Hostname     |
|:--------------|:---------------------|:-------------|
| 192.168.10.2  | HP ProCurve Switch   | JHL-SW-01    |
| 192.168.10.11 | Physical Host 01     | JHL-HV-01    |
| 192.168.10.12 | Physical Host 02     | JHL-HV-02    |
| 192.168.10.13 | Physical Host 03     | JHL-HV-03    |
| 192.168.10.14 | Physical Host 04     | JHL-HV-04    |
| 192.168.10.15 | Physical Host 05     | JHL-HV-05    |
| 192.168.10.16 | Physical Host 06     | JHL-HV-06    |
| 192.168.10.17 | Physical Host 07     | JHL-HV-07    |
| 192.168.10.18 | Physical Host 08     | JHL-HV-08    |

## VLAN 20 - Servers (Virtual Machines)
*Subnet: 192.168.20.0/24 | Gateway: 192.168.20.1*

| IP Address    | VM Name     | Role                 | Host       |
|:--------------|:------------|:---------------------|:-----------|
| 192.168.20.1  | JHL-SOPHOS  | Firewall/Gateway     | JHL-HV-01  |
| 192.168.20.11 | JHL-DC-01   | Domain Controller 1  | JHL-HV-03  |
| 192.168.20.12 | JHL-DC-02   | Domain Controller 2  | JHL-HV-04  |
| 192.168.20.20 | JHL-FS-01   | File Server (NAS)    | JHL-HV-05  |
