# Network Cleanliness Backlog

This register tracks configuration cleanup, hardening, and documentation work for the home lab. GitHub Issues are the source of truth for execution status and validation evidence; this document preserves prioritization, scope, and sequencing in version control.

## Priority model

| Priority | Meaning |
|---|---|
| P0 | Correctness or credential exposure; handle first |
| P1 | Material security-boundary or architecture cleanup |
| P2 | Operational hygiene, consistency, and maintainability |

## Active backlog

| Priority | Issue | Area | Desired outcome |
|---|---|---|---|
| P0 | [#1 Correct Unbound overrides and interface bindings](https://github.com/nicholasmbattista23/home-lab-infrastructure/issues/1) | DNS / VPN | Correct the malformed host record, serve VPN DNS, and stop binding DNS to WAN |
| P0 | [#2 Rotate exposed EX2200 authentication and SNMP credentials](https://github.com/nicholasmbattista23/home-lab-infrastructure/issues/2) | Credentials | Replace exposed authentication material and verify monitoring |
| P1 | [#3 Restrict OPNsense management listeners](https://github.com/nicholasmbattista23/home-lab-infrastructure/issues/3) | Management plane | Bind SSH and web administration only to approved trusted interfaces |
| P1 | [#4 Decide the disposition of the legacy untagged LAN](https://github.com/nicholasmbattista23/home-lab-infrastructure/issues/4) | Architecture | Document and secure the legacy LAN or remove it safely |
| P1 | [#5 Replace broad HOME, LAB, and SERVERS allowances](https://github.com/nicholasmbattista23/home-lab-infrastructure/issues/5) | Firewall policy | Move from broad access to a tested alias-based inter-zone policy |
| P1 | [#6 Validate WireGuard destination scope and internal DNS](https://github.com/nicholasmbattista23/home-lab-infrastructure/issues/6) | Remote access | Restrict remote clients to documented networks and validate DNS |
| P2 | [#7 Review WAN exposure and NAT inventory](https://github.com/nicholasmbattista23/home-lab-infrastructure/issues/7) | Edge security | Maintain an intentional inventory of every publicly reachable service |
| P2 | [#8 Normalize Kea reservations and address allocation](https://github.com/nicholasmbattista23/home-lab-infrastructure/issues/8) | DHCP / IPAM | Establish a consistent reservation convention and prevent collisions |
| P2 | [#9 Clean EX2200 descriptions and validate physical paths](https://github.com/nicholasmbattista23/home-lab-infrastructure/issues/9) | Physical layer | Make interface descriptions and topology match live cabling |
| P2 | [#10 Harden unused EX2200 ports and tracing](https://github.com/nicholasmbattista23/home-lab-infrastructure/issues/10) | Switch hygiene | Secure unused ports and remove unnecessary high-verbosity tracing |

## Recommended sequence

1. Correct DNS and rotate exposed credentials.
2. Confirm console recovery and the legacy-LAN purpose.
3. Restrict management listeners.
4. Validate WireGuard DNS and destination scope.
5. Build the intended inter-zone traffic matrix.
6. Tighten firewall policy one zone at a time.
7. Normalize DHCP/IPAM and review WAN exposure.
8. Finish switch description, unused-port, and tracing cleanup.

## Change discipline

For each cleanup item:

1. Capture the current state without committing secrets or unique hardware identifiers.
2. Define expected behavior and a rollback method.
3. Make one bounded change.
4. Validate permitted traffic and expected denials.
5. Attach sanitized evidence to the issue.
6. Update the relevant architecture or runbook document.
7. Close the issue only after acceptance criteria pass.

## Publication safety

Do not commit:

- Passwords, password hashes, keys, tokens, or SNMP communities
- Public WAN addresses or private VPN endpoints
- MAC addresses, chassis serial numbers, or VM UUIDs
- Raw firewall or switch configuration exports
- Personally identifying VPN peer names
- Screenshots containing any of the above

Use role-based aliases, private network ranges, and generic client identifiers in public documentation.
