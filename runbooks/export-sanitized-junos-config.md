# Export a Sanitized Junos Configuration

Do not paste or commit raw `show configuration` output. It contains password hashes and can include communities, keys, and other authentication data.

## Preferred collection command

Generate set-format output while excluding common secret-bearing lines:

```text
show configuration | display set | except "encrypted-password|community|authentication-key|secret|private-key|ssh-rsa|ssh-ed25519"
```

This filter is defensive, not exhaustive.

## Review before sharing

Search the resulting output for:

```text
password
community
authentication
key
secret
token
radius
tacacs
snmp
public
private
```

Also remove or replace:

- Public IP addresses
- Customer or employer names
- Serial numbers
- MAC addresses
- Usernames that identify people
- VPN endpoints
- Unrelated configuration

## Safe documentation workflow

1. Capture the filtered set-format configuration locally.
2. Copy only the sections required for the document.
3. Replace secret values with explicit placeholders.
4. Compare the sanitized output against the live configuration for structural accuracy.
5. Commit only the sanitized file.
6. Delete the temporary raw capture.

## Exposure response

If a password hash, community, key, or token is pasted or committed:

1. Treat it as exposed.
2. Rotate or revoke it.
3. Remove it from the document or repository.
4. If committed, remove it from Git history.
5. Verify authentication and monitoring using the replacement credential.
