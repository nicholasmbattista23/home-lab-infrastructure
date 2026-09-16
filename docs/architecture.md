# Architecture

## Overview

The lab uses a virtualized firewall and service platform with a managed access switch.

- OPNsense provides internal VLAN gateways, inter-VLAN policy, NAT, DHCP, Unbound DNS, NTP, and WireGuard.
- A Juniper EX2200 provides VLAN-aware switching, RSTP, a management SVI, and a separate routed lab-transit interface.
- A Dell PowerEdge R830 running Proxmox hosts infrastructure and application VMs.
- An ASUS router operates only as a wireless access point.
- Zabbix monitors the hypervisor, Linux hosts, Docker workloads, network devices, and selected application endpoints.

The historical Eero and flat `10.0.0.0/8` topology are retired and intentionally excluded from the current design.

## Logical roles

| Layer | Component | Responsibility |
|---|---|---|
| Edge | OPNsense | WAN termination, firewall policy, NAT, VPN |
| Internal routing | OPNsense | VLAN gateways and inter-VLAN routing |
| Access | Juniper EX2200 | VLAN access/trunk ports, RSTP, management SVI |
| Lab transit | Juniper EX2200 / Cisco C1921 | Separate routed lab path |
| Compute | Proxmox on Dell R830 | VM lifecycle and virtual networking |
| Wireless | ASUS AP | HOME VLAN wireless access |
| Observability | Zabbix | Metrics, availability, discovery, and alerting |
| Services | Linux and Docker VMs | Media, game, storage, and automation workloads |

## Proxmox-to-switch paths

The verified EX2200 configuration documents three distinct R830 links:

| Switch port | R830 role |
|---|---|
| `ge-0/0/1` | Proxmox management access on MGMT |
| `ge-0/0/7` | OPNsense WAN on HOME-LAN VLAN 100 |
| `ge-0/0/12` | OPNsense internal VLAN trunk |
| `ge-0/0/13` | General VM VLAN trunk |

This separation keeps the hypervisor management plane distinct from firewall WAN and VM/service traffic.

## Management and routed state

The EX2200 management SVI is on VLAN 40. Its internal summary route points toward OPNsense. A separate routed interface connects to a Cisco C1921 lab-transit network and supplies the switch default route.

The switch therefore operates primarily as the internal Layer-2 access platform, but it is not strictly Layer 2-only.

## Management plane

Infrastructure management is centralized on VLAN 40. Public management access is prohibited. Remote administration enters through WireGuard and is controlled by OPNsense policy.

Management targets include the hypervisor, switch, monitoring server, firewall UI, and server administration endpoints. Application traffic does not implicitly grant management access.

## Naming and DNS

The internal namespace is `lab.home.arpa`. Stable infrastructure and server systems receive deliberate DNS records. Public DNS is not used to expose management services.

## Related documents

- [Juniper EX2200](juniper-ex2200.md)
- [Logical topology](../diagrams/logical-topology.md)
- [Physical topology](../diagrams/physical-topology.md)
- [Network segmentation](network-segmentation.md)
- [Security model](security-model.md)
