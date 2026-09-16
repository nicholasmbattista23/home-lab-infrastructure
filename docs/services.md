# Services

This page catalogs implemented service roles without including credentials, private data, or unredacted application configuration.

## Infrastructure

| Service | Host/role | Function |
|---|---|---|
| OPNsense | Virtual firewall | Routing, firewall, NAT, DHCP, DNS, NTP, WireGuard |
| Proxmox | `hypervisor-01` | Virtualization and VM networking |
| Zabbix | `monitor-01` | Infrastructure and application monitoring |
| Juniper EX2200 | Access switch | VLAN switching, RSTP, SNMP |
| ASUS AP | Wireless access | HOME VLAN wireless access |

## Media and storage

| Service | Host/role | Function |
|---|---|---|
| Plex | `media-01` | Media library and streaming |
| Immich | `media-01` | Photo and video management |
| Samba | `media-01` | Network file access |
| qBittorrent | `media-ingest-01` | Download client |
| Sonarr/Radarr | `media-ingest-01` | Library automation |
| Prowlarr | `media-ingest-01` | Indexer management |
| Byparr/Privoxy | `media-ingest-01` | Supporting proxy services |

Repository examples will describe container relationships and storage mounts without publishing API keys, credentials, VPN configuration, media, or indexer details.

## Game servers

| Service | Host |
|---|---|
| Palworld dedicated server | `palworld-01` |
| Valheim dedicated server | `valheim-01` |

Game server documentation will focus on VM sizing, systemd or container lifecycle, network policy, backups, and monitoring rather than player data.

## Separation of concerns

The remote-access WireGuard service is distinct from any application-specific outbound VPN. Remote administration and application egress must not share peers, credentials, or policy assumptions.
