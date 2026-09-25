# Debian Server Baseline Deployment

## Purpose

Use this runbook after creating a new Debian VM to bring it to a consistent home-lab baseline before installing an application or service.

This procedure standardizes:

- Host identity and basic network checks
- Administrative user access
- Base packages and updates
- QEMU Guest Agent for Proxmox guests
- SSH service state
- Time synchronization
- A consistent color-coded Bash prompt
- Final validation evidence

The application-specific configuration comes after this baseline is complete.

## Prerequisites

- Debian installation is complete.
- The VM has the intended vCPU, memory, disk, bridge/VLAN, and boot settings.
- The VM has network reachability.
- The intended hostname is known.
- An administrative user exists or the root password is available.

Use placeholders in public documentation rather than live credentials or sensitive addressing.

## Deployment variables

Set the intended values before starting:

```text
HOSTNAME=<server-name>
ADMIN_USER=<admin-user>
DOMAIN=lab.home.arpa
```

Example hostname convention:

```text
<role>-01
```

## 1. Enter a proper root login shell

When escalating from a normal account, use a login shell so root receives the correct environment and PATH:

```bash
su -
```

Validate:

```bash
whoami
pwd
echo "$PATH"
```

Expected:

```text
root
/root
```

## 2. Set and verify the hostname

Set the hostname when required:

```bash
hostnamectl set-hostname <HOSTNAME>
```

Verify:

```bash
hostnamectl
hostname -f
```

If local hostname resolution is required before DNS registration, ensure `/etc/hosts` contains an appropriate local entry for the host. Do not overwrite resolver-managed configuration solely to force an FQDN.

## 3. Update Debian and install baseline packages

Refresh package metadata and apply current updates:

```bash
apt update
apt full-upgrade -y
```

Install the common home-lab baseline:

```bash
apt install -y \
  sudo \
  openssh-server \
  qemu-guest-agent \
  curl \
  ca-certificates \
  git \
  vim \
  htop \
  dnsutils \
  iproute2
```

Reboot if the update installed a new kernel or otherwise requires one:

```bash
[ -f /var/run/reboot-required ] && cat /var/run/reboot-required
```

## 4. Prepare the administrative user

If the intended administrative account already exists, verify it:

```bash
id <ADMIN_USER>
```

If it does not exist, create it:

```bash
adduser <ADMIN_USER>
```

Add the account to the `sudo` group:

```bash
usermod -aG sudo <ADMIN_USER>
```

Verify group membership:

```bash
id <ADMIN_USER>
```

Before ending the root session, test the account in a separate session and confirm sudo works:

```bash
sudo -v
sudo whoami
```

Expected:

```text
root
```

## 5. Enable SSH

Enable and start OpenSSH:

```bash
systemctl enable --now ssh
systemctl status ssh --no-pager
```

Verify the listener:

```bash
ss -lntp | grep ':22'
```

Do not disable password authentication until key-based access has been tested from another session. Root SSH login should remain disabled unless a specific lab workflow requires it.

## 6. Enable the QEMU Guest Agent

For Proxmox-hosted Debian VMs:

```bash
systemctl enable --now qemu-guest-agent
systemctl status qemu-guest-agent --no-pager
```

Confirm the VM also has **QEMU Guest Agent** enabled in the Proxmox VM options.

## 7. Validate time synchronization

Check current time state:

```bash
timedatectl
```

Confirm the system clock is synchronized and the expected timezone is configured.

Useful checks:

```bash
timedatectl status
systemctl status systemd-timesyncd --no-pager 2>/dev/null || true
```

Do not proceed with monitoring, TLS-sensitive services, or clustered applications while time is incorrect.

## 8. Apply the standard color-coded Bash prompt

The lab prompt uses color as a quick privilege and location cue:

- Normal user/host: **green**
- Root user/host: **red**
- Current working directory: **blue**

This preserves the familiar prompt structure:

```text
user@host:/current/directory$
root@host:/current/directory#
```

Back up the system Bash configuration first:

```bash
cp -a /etc/bash.bashrc /etc/bash.bashrc.pre-lab-prompt
```

Append the managed prompt block only if it is not already present:

```bash
grep -q 'BEGIN HOME LAB PROMPT' /etc/bash.bashrc || cat >> /etc/bash.bashrc <<'EOF'

# BEGIN HOME LAB PROMPT
# Color-coded interactive prompt: green normal user, red root, blue cwd.
case $- in
  *i*) ;;
    *) return ;;
esac

if [ "$(id -u)" -eq 0 ]; then
    PS1='\[\e[1;31m\]\u@\h\[\e[0m\]:\[\e[1;34m\]\w\[\e[0m\]# '
else
    PS1='\[\e[1;32m\]\u@\h\[\e[0m\]:\[\e[1;34m\]\w\[\e[0m\]\$ '
fi
# END HOME LAB PROMPT
EOF
```

Start a fresh shell or reload the configuration:

```bash
source /etc/bash.bashrc
```

Validate both a normal-user shell and a root shell. Root should be visually distinct in red.

## 9. Baseline network validation

Record the host identity and addresses:

```bash
hostname
hostname -f
ip -br addr
ip route
```

Validate the default gateway and DNS path:

```bash
ping -c 4 <DEFAULT_GATEWAY>
getent hosts <KNOWN_INTERNAL_NAME>
```

Validate external package/DNS reachability when the server is intended to have it:

```bash
getent hosts deb.debian.org
```

## 10. Final service validation

Run the baseline checks:

```bash
printf '%s\n' '--- identity ---'
hostnamectl

printf '%s\n' '--- network ---'
ip -br addr
ip route

printf '%s\n' '--- ssh ---'
systemctl is-enabled ssh
systemctl is-active ssh

printf '%s\n' '--- guest agent ---'
systemctl is-enabled qemu-guest-agent
systemctl is-active qemu-guest-agent

printf '%s\n' '--- time ---'
timedatectl status

printf '%s\n' '--- storage ---'
df -h /

printf '%s\n' '--- memory ---'
free -h
```

## Completion criteria

The Debian baseline is complete when:

- Hostname is correct.
- Intended network interface and route are present.
- DNS resolution works as designed.
- Debian updates have been applied.
- Administrative user exists and sudo is verified.
- SSH is enabled and reachable.
- QEMU Guest Agent is active for Proxmox VMs.
- Time synchronization is healthy.
- Normal-user and root prompts are visually distinct.
- Root filesystem and memory state are reasonable.
- Any required reboot has been completed.

## Next steps

After this baseline, continue with the service-specific runbook, such as:

- Zabbix Agent 2 onboarding
- Docker/container-host preparation
- Application deployment
- Service-specific firewall and monitoring rules

Do not install application-specific packages until the baseline validation is clean.