# Security

Security combines host access controls, firewall policy, security tooling and
operational notifications.

## Controls and Status

| Component | Current state |
| --- | --- |
| UFW | Default incoming deny, outgoing allow and routed deny; selected ports permitted |
| SSH | Effective access restrictions listed below |
| Unattended upgrades | Present for automated update handling |
| Lynis | Deployed for security auditing |
| rkhunter | Deployed for security checks |
| CrowdSec | Deployed; active enforcement not verified |

## Effective SSH Configuration

Effective settings checked through `sshd -T`:

| Setting | Effective value |
| --- | --- |
| `PermitRootLogin` | `no` |
| `PubkeyAuthentication` | `yes` |
| `PasswordAuthentication` | `yes` |
| `MaxAuthTries` | `3` |
| `X11Forwarding` | `no` |

Root login and X11 forwarding are disabled, and authentication attempts are
limited. Both public-key and password authentication remain enabled. A
cloud-init configuration overrides the main SSH configuration for password
authentication, so the effective state is not key-only access.

## Firewall and Access Boundaries

UFW policy is interpreted alongside Docker-managed port-publishing rules
and upstream router/NAT configuration.

See [networking](networking.md) for this interaction and the deployed VPN services.

## Operational Integration

Security scan/log results feed n8n for status classification and Telegram
notifications. See [operations](operations.md) for the workflow roles and
execution history.
