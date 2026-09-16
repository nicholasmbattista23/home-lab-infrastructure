# Add a Linux Server to Zabbix

## Prerequisites

- The server has a stable hostname and address.
- The monitoring server can reach TCP/10050.
- The server can reach the monitoring server for active checks.
- Time synchronization is healthy.
- The correct OS/application templates are known.

## Procedure

1. Install Zabbix Agent 2 from the appropriate vendor repository.
2. Edit the Agent 2 configuration:

   ```ini
   Server=<ZABBIX_SERVER_IP>
   ServerActive=<ZABBIX_SERVER_IP>
   Hostname=<EXACT_ZABBIX_HOSTNAME>
   ```

3. Enable and start the service:

   ```bash
   sudo systemctl enable --now zabbix-agent2
   sudo systemctl status zabbix-agent2 --no-pager
   ```

4. Confirm the listener:

   ```bash
   ss -lntp | grep ':10050'
   ```

5. Test a local item:

   ```bash
   zabbix_agent2 -t agent.ping
   ```

6. Create the matching host in Zabbix.
7. Apply the OS template and any application-specific templates.
8. Confirm new data and discovery results.
9. Record the onboarding evidence.

## Validation

A host is not complete until:

- Agent 2 is enabled and active
- The Zabbix hostname matches exactly
- TCP/10050 is restricted to the monitoring source
- Latest Data shows current values
- Expected discovery rules complete
- No unsupported-item flood is present

## Troubleshooting

Check:

```bash
journalctl -u zabbix-agent2 --since '-15 minutes'
zabbix_agent2 -T
getent hosts <ZABBIX_SERVER_NAME>
```

Do not paste tokens, PSKs, or full production configuration into public issues.
