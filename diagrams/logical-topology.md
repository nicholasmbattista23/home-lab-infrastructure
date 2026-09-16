# Logical Topology

```mermaid
flowchart TB
    WAN["Upstream / WAN segment
VLAN 100"] --> FW["OPNsense"]

    WG["WireGuard clients
Routed VPN network"] --> FW

    FW --> HOME["HOME
VLAN 10"]
    FW --> LAB["LAB
VLAN 20"]
    FW --> SERVERS["SERVERS
VLAN 30"]
    FW --> MGMT["MGMT
VLAN 40"]
    FW --> GUEST["GUEST
VLAN 50"]
    FW --> IOT["IOT
VLAN 60"]

    HOME --> AP["ASUS AP"]
    SERVERS --> APP["Media and game services"]
    MGMT --> PVE["Proxmox"]
    MGMT --> SW["Juniper EX2200"]
    MGMT --> ZBX["Zabbix"]

    SW -->|"Routed private transit"| C1921["Cisco C1921"]
```

## Policy summary

- OPNsense is the internal Layer-3 and security boundary.
- The EX2200 supplies VLAN-aware switching plus management and lab-transit Layer-3 interfaces.
- Management services are reachable only from approved trusted or VPN sources.
- Guest and IoT networks do not receive implicit access to HOME, SERVERS, or MGMT.
- The WireGuard VPN is routed by OPNsense and is not extended as an EX2200 switching VLAN.
