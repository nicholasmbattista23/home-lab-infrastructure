# Repository Security Policy

This repository is intended to be safe for public review. It documents architecture and engineering practices without providing authentication material or direct access to the environment.

## Never commit

- Passwords, password hashes, recovery codes, or credential exports
- WireGuard private keys, preshared keys, peer QR codes, or complete tunnel configs
- SSH private keys or deploy keys
- API tokens, session cookies, application secrets, or database credentials
- SNMP communities
- Public WAN addresses or dynamic-DNS credentials
- Raw OPNsense XML backups
- Raw device configuration backups
- MAC-address inventories, serial numbers, or asset tags
- Packet captures containing user traffic
- Unredacted screenshots containing names, email addresses, tokens, keys, or browser sessions
- Real `.env` files or application databases
- Media, personal photos, or family archive content

## Safe contribution pattern

1. Copy the operational configuration into a temporary working location.
2. Replace secrets and identifying values with clear placeholders.
3. Remove unused or unrelated configuration.
4. Review the diff manually.
5. Run a secret scan before committing.
6. Delete the temporary working copy when finished.

Use placeholders such as:

- `<ZABBIX_SERVER_IP>`
- `<API_TOKEN>`
- `<WIREGUARD_PUBLIC_KEY>`
- `<UNIQUE_HOSTNAME>`

Private RFC 1918 subnet design may be documented deliberately, but public WAN information must remain excluded.

## Repository scope

This repository contains no employer, customer, or production-network information. Work-related tooling and inventories belong in separate private repositories.
