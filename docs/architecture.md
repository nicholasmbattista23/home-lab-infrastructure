# Architecture

## Overview

The lab uses a virtualized firewall and service platform with a managed Layer-2 access switch.

- OPNsense provides Layer-3 gateways, inter-VLAN policy, NAT, DHCP, Unbound DNS, NTP, and WireGuard.
- A Juniper EX2200 provides VLAN-aware Layer-2 switching and RSTP.
- A Dell PowerEdge R830 running Proxmox hosts infrastructure and application VMs.
- An ASUS router operates only as a wireless access point.
- Zabbix monitors the hypervisor, Linux hosts, Docker workloads, network devices, and selected application endpoints.

The historical Eero and flat `10.0.0.0/8` topology are retired and intentionally excluded from the current design.

## Logical roles

| Layer | Component | Responsibility |
|---|---|---|
| Edge | OPNsense | WAN termination, firewall policy, NAT, VPN |
| Routing | OPNsense | VLAN gateways and inter-VLAN routing |
| Access | Juniper EX2200 | VLAN access/trunk ports and RSTP |
| Compute | Proxmox on Dell R830 | VM lifecycle and virtual networking |
| Wireless | ASUS AP | HOME VLAN wireless access |
| Observability | Zabbix | Metrics, availability, discovery, and alerting |
| Services | Linux and Docker VMs | Media, game, storage, and automation workloads |

## Management plane

Infrastructure management is centralized on VLAN 40. Public management access is prohibited. Remote administration enters through WireGuard and is controlled by OPNsense policy.

Management targets include the hypervisor, switch, monitoring server, firewall UI, and server administration endpoints. Application traffic does not implicitly grant management access.

## Compute model

Proxmox uses Linux bridges to connect the firewall and service VMs to the required physical or VLAN-aware paths. Service VMs are assigned to the minimum network scope required for their role.

The portfolio documents the bridge and VLAN model, but operational interface names and complete host configurations must be verified before reuse.

## Naming and DNS

The internal namespace is `lab.home.arpa`. Stable infrastructure and server systems receive deliberate DNS records. Public DNS is not used to expose management services.

## Related documents

- [Logical topology](../diagrams/logical-topology.md)
- [Physical topology](../diagrams/physical-topology.md)
- [Network segmentation](network-segmentation.md)
- [Security model](security-model.md)
