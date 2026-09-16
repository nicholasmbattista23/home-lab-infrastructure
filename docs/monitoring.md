# Monitoring

## Platform

`monitor-01` runs the Zabbix monitoring platform with:

- Zabbix Server
- PostgreSQL
- Nginx
- PHP-FPM
- Zabbix Agent 2
- SNMP tooling

## Coverage

| Target | Method | Purpose |
|---|---|---|
| Proxmox | HTTPS API | Node, VM, storage, and platform health |
| Linux servers | Zabbix Agent 2 | OS, filesystem, process, and service metrics |
| Docker hosts | Agent 2 Docker discovery | Container discovery and health |
| Juniper EX2200 | SNMP | Interface, link, error, traffic, and system metrics |
| OPNsense | SNMP/service checks | Firewall and interface visibility |
| Plex | HTTP endpoint check | Application availability |
| Samba | TCP/service check | File-service availability |

## Agent onboarding standard

Each Linux host receives:

- A unique hostname matching its Zabbix host object
- `Server` and `ServerActive` pointed to the monitoring server
- Agent 2 enabled at boot
- TCP/10050 permitted only from the monitoring source
- A passive item test before the host is considered monitored
- The correct OS and application templates

See [Add a Linux server to Zabbix](../runbooks/add-linux-server-to-zabbix.md).

## Monitoring principles

- Monitor service behavior, not only ICMP reachability.
- Alert on actionable conditions.
- Retain enough history to distinguish transient faults from trends.
- Validate templates after host or address changes.
- Treat missing data as an operational signal.

## Status

Core monitoring is operational. Alert thresholds, dashboards, and exportable sanitized templates will continue to be added as they are validated.
