# Project Status and Next-Session Handoff

**Checkpoint date:** 2026-09-16  
**Repository state:** Documentation work paused cleanly; lab remains operational.

## Completed

- Established the sanitized home-lab portfolio structure.
- Documented the logical and physical topology.
- Documented VLANs, trust zones, core services, monitoring, and security principles.
- Added the verified Juniper EX2200 state and sanitized set-format example.
- Added a Junos configuration-sanitization runbook.
- Created the version-controlled [network cleanliness backlog](network-cleanliness.md).
- Opened ten GitHub Issues with work checklists and acceptance criteria.
- Removed server manufacturer, hardware model, and model-bearing hypervisor hostname references from the current branch.
- Verified that the current branch uses generic `Proxmox hypervisor`, `hypervisor-01`, and `Hypervisor NIC` terminology.

## OPNsense evidence collected

The following current-state evidence has been reviewed but has not yet been consolidated into a dedicated OPNsense document:

- OPNsense software version and virtual-machine resource profile
- Two-interface virtual firewall architecture
- VLAN interfaces 10 through 60
- Routed WireGuard network on VLAN/subnet 70
- Legacy untagged LAN presence
- Connected and local routing table
- Alias-based firewall objects
- Interface rule sets for HOME, LAB, SERVERS, MGMT, GUEST, IOT, and WireGuard
- WAN pass rules
- Destination NAT for Plex and qBittorrent peer traffic
- Automatic/default outbound source NAT
- Kea DHCP scopes and reservations
- Unbound host overrides and listener inventory
- WireGuard listener and single-client proof of concept
- NTP configuration and healthy peer synchronization
- Listening-service inventory

Raw public addressing, MAC addresses, VM UUIDs, VPN identifiers, credentials, and hardware identifiers must remain excluded from committed documentation.

## Verified OPNsense policy summary

| Zone | Current effective behavior |
|---|---|
| HOME | Broad trusted access |
| LAB | Broad access pending policy refinement |
| SERVERS | DNS to firewall, blocked from MGMT, otherwise broad access |
| MGMT | Trusted management access; NTP served locally |
| GUEST | DNS to firewall, RFC1918 blocked, internet permitted |
| IOT | DNS to firewall, RFC1918 blocked, internet permitted |
| WireGuard | DNS and access to the approved internal-network alias |
| WAN | Plex, WireGuard, and qBittorrent peer traffic only |

## Known follow-up items

Execution is tracked in [GitHub Issues](https://github.com/nicholasmbattista23/home-lab-infrastructure/issues). Highest-priority items are:

1. [Correct Unbound overrides and interface bindings](https://github.com/nicholasmbattista23/home-lab-infrastructure/issues/1).
2. [Rotate exposed EX2200 authentication and SNMP credentials](https://github.com/nicholasmbattista23/home-lab-infrastructure/issues/2).
3. [Restrict OPNsense management listeners](https://github.com/nicholasmbattista23/home-lab-infrastructure/issues/3).
4. [Determine the purpose or retirement plan for the legacy untagged LAN](https://github.com/nicholasmbattista23/home-lab-infrastructure/issues/4).
5. [Replace broad firewall allowances with a tested traffic matrix](https://github.com/nicholasmbattista23/home-lab-infrastructure/issues/5).
6. [Validate WireGuard DNS and destination scope](https://github.com/nicholasmbattista23/home-lab-infrastructure/issues/6).

## Inputs still needed

Before calling the OPNsense section fully validated:

- Confirm the three intended members of `VPN_INTERNAL_NETWORKS`.
- Confirm whether the untagged legacy LAN is intentional, transitional, or retired.
- Correct and retest the malformed `media-ingest-01` Unbound override.
- Add the WireGuard interface to Unbound and remove WAN from its listener set, then validate.
- Decide the approved interfaces for web and SSH management.

## Exact next-session starting point

Do not repeat the inventory collection.

1. Review the outstanding decisions above.
2. Create `docs/opnsense.md` from the evidence already collected.
3. Add a sanitized OPNsense service-flow and trust-boundary diagram.
4. Add an OPNsense backup/restore and validation runbook.
5. Update the README and architecture documents to link the finished OPNsense section.
6. Attach sanitized validation evidence to the applicable issues and close only those whose acceptance criteria pass.

## Later documentation phases

After OPNsense:

1. Hypervisor bridge, VM-network, startup-order, backup, and recovery documentation.
2. Expanded Zabbix monitoring architecture and onboarding lifecycle.
3. Media, game-server, and container service documentation.
4. Lab-wide backup and disaster-recovery plan.
5. Repository automation for secret scanning, Markdown/link checks, and configuration-sanitization checks.
6. Final privacy and Git-history review before any public release.

## Git-history note

The current branch no longer displays the server manufacturer or hardware model. Earlier commits still contain the former terminology. Rewriting Git history is intentionally deferred because it is disruptive and should be performed only as a deliberate step before publication if complete historical removal is required.
