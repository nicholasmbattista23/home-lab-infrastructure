# Physical and Virtual Topology

```mermaid
flowchart LR
    ISP["ISP handoff"] --> PVEWAN["R830 physical WAN path"]
    PVEWAN --> FWVM["OPNsense VM"]
    FWVM --> TRUNK["Internal VLAN trunk"]
    TRUNK --> EX["Juniper EX2200"]
    EX --> AP["ASUS AP"]
    EX --> PHYS["Physical servers/devices"]

    PVE["Proxmox on R830"] --> FWVM
    PVE --> MON["monitor-01"]
    PVE --> INGEST["media-ingest-01"]
    PVE --> GAME1["palworld-01"]
    PVE --> GAME2["valheim-01"]
```

This diagram intentionally omits interface identifiers, MAC addresses, and public-WAN details. The live bridge/interface configuration remains an operational artifact and must be sanitized before publication.
