# Validate VLAN Connectivity

## Client information

Capture the assigned address, prefix, gateway, DNS server, interface, and VLAN attachment.

## Validation sequence

1. Confirm local interface and route state.
2. Ping the VLAN gateway.
3. Query the intended internal DNS resolver.
4. Resolve an internal `lab.home.arpa` record.
5. Resolve a public hostname when Internet access is expected.
6. Test only the explicitly approved cross-VLAN services.
7. Confirm prohibited destinations remain blocked.
8. Review OPNsense logs for the tested source and destination.
9. Confirm the endpoint appears in monitoring.

## Example Linux commands

```bash
ip address
ip route
ping -c 4 <VLAN_GATEWAY>
getent hosts <INTERNAL_NAME>.lab.home.arpa
curl -I http://<APPROVED_SERVICE_IP>:<PORT>/
```

## Example PowerShell commands

```powershell
Get-NetIPConfiguration
Test-Connection <VLAN_GATEWAY> -Count 4
Resolve-DnsName <INTERNAL_NAME>.lab.home.arpa
Test-NetConnection <APPROVED_SERVICE_IP> -Port <PORT>
```

## Evidence matrix

| Test | Expected |
|---|---|
| DHCP/static addressing | Correct subnet, gateway, and DNS |
| Gateway | Reachable |
| Internal DNS | Correct forward/reverse answer |
| Approved application | Reachable on documented port |
| MGMT from untrusted VLAN | Blocked |
| Internet | Allowed or denied according to role |
| Monitoring | Endpoint/service visible |

Do not replace firewall policy with broad temporary permits unless the test is controlled, time-bounded, and immediately removed.
