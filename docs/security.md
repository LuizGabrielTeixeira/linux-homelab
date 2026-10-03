# Security

Security combines host access controls, firewall policy, security tooling and
operational notifications. Claims below distinguish effective configuration
from deployed software whose current enforcement is not verified.

## Controls and Status

| Component | Documented state |
| --- | --- |
| UFW | Default incoming deny, outgoing allow and routed deny; selected ports permitted |
| SSH | Effective access restrictions listed below |
| Unattended upgrades | Present for automated update handling |
| Lynis | Deployed for security auditing |
| rkhunter | Deployed for security checks |
| CrowdSec | Deployed; active enforcement not currently treated as verified |

Tool deployment is not a claim that the host is free of vulnerabilities or that
every scan, update or enforcement action succeeds.

## Effective SSH Configuration

The supplied effective settings, verified through `sshd -T`, are:

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

UFW policy must be interpreted alongside Docker-managed port-publishing rules
and upstream router/NAT configuration. The public documentation does not list
reachable endpoints or reproduce full firewall output.

See [networking](networking.md) for this interaction and the deployed VPN services.

## Operational Integration

Security scan/log results feed n8n for status classification and Telegram
notifications. This records the automation architecture, not a recent test
result for every scan or notification path.

## Public Documentation Boundary

Credentials, authentication material, VPN peers, domains, addresses and personal
paths are excluded. Generic host and storage labels explain controls without
publishing access details. No raw audit output or full workflows are included.
