# Security Model

## Objectives

- Prevent direct Internet exposure of management services
- Limit lateral movement between client, server, IoT, printer, and management networks
- Require authenticated remote access
- Keep secrets out of source control
- Preserve enough documentation to rebuild and audit the environment

## Trust boundaries

### WAN to internal

Inbound traffic is denied unless explicitly published. Infrastructure management interfaces are never directly exposed.

### VPN to internal

WireGuard is the remote-administration entry point. Each physical client receives its own peer and keypair. A phone peer is never reused for a laptop or another device.

VPN access is still filtered by firewall policy; tunnel establishment does not imply unrestricted access.

### User and server networks

Trusted client systems may reach approved application services. Server systems do not receive broad access to the management plane.

### Restricted-device networks

IoT and printer segments are treated as lower trust. Access is limited to required DNS, time, update, printing, or management workflows.

## Administrative protections

- Separate management VLAN
- Default-deny inter-VLAN posture
- Named firewall aliases and port objects
- Per-device WireGuard peers
- No public Proxmox, iDRAC, Juniper, Zabbix, or OPNsense administration
- Read-only SNMP for monitoring
- API tokens scoped to minimum required roles
- Configuration backups stored outside this public repository

## Source-control protections

Only sanitized examples belong here. The repository ignore rules are defensive, not a substitute for reviewing every commit.

If a secret is committed, removing it from the current file is insufficient. Revoke or rotate the secret and remove it from Git history.
