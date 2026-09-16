# Juniper EX2200 Verified State

Source evidence: sanitized review of the active configuration and operational hardware/interface output captured on 2026-09-16. The most recent switch configuration commit was dated 2026-09-09.

## Platform

| Item | Value |
|---|---|
| Hostname | `EX2200-LAB-01` |
| Platform | Juniper EX2200-24T-4G |
| Junos | 12.3R12-S21 |
| Copper access ports | 24 × 10/100/1000BASE-T |
| Uplink ports | 4 × Gigabit Ethernet SFP |
| Power | 100W AC power supply |
| Primary role | VLAN access/trunk switching |
| Spanning tree | RSTP root, bridge priority 4k |
| Time source | OPNsense on the MGMT network |
| Monitoring | Read-only SNMP restricted to `monitor-01` |
| Discovery | LLDP and LLDP-MED enabled |

Chassis serial numbers, authentication hashes, and the SNMP community are deliberately excluded.

## Verified port map

| Interface | Description/role | Configured mode | VLANs/addressing | Observed link |
|---|---|---|---|---|
| `ge-0/0/0` | ASUS home Wi-Fi AP | Access | HOME | Up |
| `ge-0/0/1` | Proxmox NIC0 management | Access | MGMT | Up |
| `ge-0/0/2` | `media-01` | Access | SERVERS | Up |
| `ge-0/0/4` | Raspberry Pi Ethernet | Access | HOME | Up |
| `ge-0/0/7` | Proxmox NIC1 / labeled OPNsense WAN | Access | HOME-LAN | Down |
| `ge-0/0/11` | Stale duplicate ASUS description | Access | HOME | Down |
| `ge-0/0/12` | Proxmox NIC2 / OPNsense LAN | Trunk | HOME, LAB, SERVERS, MGMT, GUEST, IOT | Up |
| `ge-0/0/13` | Proxmox NIC3 / VM trunk | Trunk | HOME, LAB, SERVERS, MGMT, GUEST, IOT | Up |
| `ge-0/0/23` | Cisco C1921 lab transit | Routed | Private /30 transit | Up |

The link-state column is a point-in-time observation, not continuous availability evidence.

Several additional access ports are currently assigned to HOME-LAN. Unused-port hardening remains a separate task.

## VLANs

| Name | VLAN ID | Switch L3 interface |
|---|---:|---|
| HOME | 10 | — |
| LAB | 20 | — |
| SERVERS | 30 | — |
| MGMT | 40 | `vlan.40` |
| GUEST | 50 | — |
| IOT | 60 | — |
| HOME-LAN | 100 | `vlan.0` using DHCP |

The WireGuard VPN subnet is routed within OPNsense and is not currently defined as an EX2200 VLAN.

## Routing and management behavior

- MGMT uses the switch SVI on VLAN 40.
- The internal `10.10.0.0/16` summary points toward OPNsense.
- The default route points toward the Cisco lab-transit next hop.
- `ge-0/0/23` is a routed port, so the device is not strictly Layer 2-only.
- The out-of-band `me0` interface is configured for DHCP.

## Confirmed operational controls

- SSH version 2 enabled
- RSTP enabled with the switch as root
- LLDP and LLDP-MED enabled
- Storm control enabled on all switching interfaces
- IGMP snooping enabled
- NTP configured
- Read-only SNMP restricted to the monitoring server

## Findings to review

1. `ge-0/0/11` retains a duplicate ASUS description but was down during the operational snapshot; `ge-0/0/0` is the active ASUS link.
2. `ge-0/0/7` was down despite being labeled as the OPNsense-WAN path; verify whether the live WAN path moved or the description is stale.
3. Several unused ports remain enabled with generic Ethernet-switching configuration.
4. DHCP tracing is configured at maximum verbosity and may no longer be required.
5. Both `me0` and `vlan.0` use DHCP; confirm that both management paths are intentional.
6. HOME-LAN/VLAN 100 remains an upstream-access segment and should be documented separately from trusted HOME.
7. Login password hashes and the SNMP community were exposed during documentation collection and must be rotated.
8. Prefer SSH public-key administration and prohibit direct root SSH login where operationally supported.

See the [sanitized set-format example](../configs/juniper/ex2200-sanitized.set).
