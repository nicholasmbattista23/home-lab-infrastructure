# Logical Topology

```mermaid
flowchart TB
    WAN["Internet"] --> FW["OPNsense"]

    WG["WireGuard clients
VLAN 70"] --> FW

    FW --> HOME["HOME
VLAN 10"]
    FW --> LAB["GUEST / LAB
VLAN 20"]
    FW --> SERVERS["SERVERS
VLAN 30"]
    FW --> MGMT["MGMT
VLAN 40"]
    FW --> IOT["IOT
VLAN 50"]
    FW --> PRINT["PRINTERS
VLAN 60"]

    HOME --> AP["ASUS AP"]
    SERVERS --> APP["Media and game services"]
    MGMT --> PVE["Proxmox"]
    MGMT --> SW["Juniper EX2200"]
    MGMT --> ZBX["Zabbix"]
```

## Policy summary

- OPNsense is the Layer-3 and security boundary.
- The EX2200 supplies VLAN-aware Layer-2 switching.
- Management services are reachable only from approved trusted or VPN sources.
- Restricted-device networks do not receive implicit access to HOME, SERVERS, or MGMT.
