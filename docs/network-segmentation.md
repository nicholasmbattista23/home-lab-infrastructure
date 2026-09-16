# Network Segmentation

OPNsense is the policy enforcement point for traffic crossing VLAN boundaries. The EX2200 transports VLANs and supplies access ports but does not replace the firewall policy layer.

## VLAN plan

| VLAN | Name | Subnet | Trust intent |
|---:|---|---|---|
| 10 | HOME | 10.10.10.0/24 | Trusted user devices and wireless clients |
| 20 | GUEST/LAB | 10.10.20.0/24 | Temporary, guest, or test systems |
| 30 | SERVERS | 10.10.30.0/24 | Application, media, and game servers |
| 40 | MGMT | 10.10.40.0/24 | Infrastructure management |
| 50 | IOT | 10.10.50.0/24 | Restricted IoT devices |
| 60 | PRINTERS | 10.10.60.0/24 | Restricted printers and similar devices |
| 70 | VPN | 10.10.70.0/24 | Authenticated remote-access clients |

Each routed VLAN uses OPNsense as its default gateway.

## Policy model

1. Deny cross-zone traffic by default.
2. Permit DNS and required infrastructure services explicitly.
3. Permit trusted administrator sources to required management destinations.
4. Permit client-to-application flows by documented port aliases.
5. Prevent application/download systems from initiating sessions toward MGMT.
6. Keep Guest, IoT, and Printers isolated unless a specific service requires access.
7. Keep WAN management closed; remote administration uses WireGuard.

## Firewall aliases

Aliases are used to express intent and reduce repeated literal addresses and ports. Examples include:

- `NET_HOME`
- `NET_SERVERS`
- `NET_MGMT`
- `NET_VPN`
- `MEDIA_SERVERS`
- `GAME_SERVERS`
- `MGMT_INFRA`
- `ZABBIX_SERVER`
- `PLEX_SERVER`
- `PORT_ZABBIX_AGENT`
- `PORT_SNMP`
- `PORT_PLEX`
- `PORT_SMB`

See the sanitized [alias example](../configs/opnsense/firewall-aliases.example.csv).

## Validation approach

A VLAN is not considered ready solely because DHCP succeeds. Validation includes:

- Address, prefix, gateway, and DNS assignment
- Gateway reachability
- Approved inter-VLAN flows
- Expected denied flows
- Internet access where appropriate
- DNS forward and reverse lookup
- Monitoring visibility
- Firewall log review for unexpected blocks or permits
