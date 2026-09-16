# Build History

This timeline captures major engineering milestones. It is not a complete change log.

## 2026-08-27 — Hypervisor rebuild

- Rebuilt the Dell PowerEdge R830 from ESXi to Proxmox
- Established the `pve-r830-01` naming convention
- Validated management access and RAID-backed installation

## 2026-08-30 — Routed network cutover

- Introduced OPNsense as the firewall and routed gateway
- Established the VLAN-based addressing model
- Validated DHCP, routing, NAT, and Internet access
- Transitioned away from the historical flat `10.0.0.0/8` design

## 2026-08-31 — Core network services

- Established internal DNS using `lab.home.arpa`
- Validated OPNsense NTP service
- Implemented WireGuard remote access
- Configured the EX2200 as RSTP root
- Validated SNMP monitoring
- Began media and Docker service deployment

## 2026-09-01 — Service and monitoring expansion

- Added media-ingest and media-service roles
- Expanded Linux and Docker monitoring
- Documented firewall aliases and service flows
- Established the current server and management inventory

## 2026-09-09 — Wireless uplink fault isolation

- Investigated repeated carrier transitions on the ASUS AP uplink
- Confirmed 1 Gbps full-duplex operation with no CRC errors
- Moved the unchanged AP/cable to a known-good switch port
- Cleared counters and observed zero new carrier transitions during the validation window
- Classified the previous Eero references as retired/stale

## Documentation rule

Planned work is labeled as planned. Historical configurations are not presented as current state.
