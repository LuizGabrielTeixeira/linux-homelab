# Networking

Networking combines a private LAN, an upstream router, Docker networks and
infrastructure services for DNS, reverse proxying and VPN remote access.

## Connectivity Roles

| Component | Role |
| --- | --- |
| AdGuard Home | DNS service, using host networking |
| Nginx Proxy Manager | Reverse proxy service on `nginx-proxy` |
| WireGuard / wg-easy | VPN access on its project bridge |
| OpenVPN | VPN access on Docker's default bridge |
| Upstream router / NAT | Determines routing and forwarding toward the host |

Nginx Proxy Manager provides an entry point for proxied services. WireGuard and
OpenVPN provide remote access; their client routing policies determine which
destinations a VPN client can reach.

## Service-Oriented Docker Separation

Reverse proxy, database and monitoring components use separate networks.
This organizes connectivity around service relationships rather than placing
every container on one shared bridge.

n8n intentionally joins both `nginx-proxy` and `database` to reach the components
it needs. Other projects have their own networks.

AdGuard Home and Netdata share the host network namespace. Their listeners
must be considered as host-network listeners rather than bridge-published ports.
OpenVPN and WireGuard use different bridge arrangements.

See the [Docker network diagram](../diagrams/docker-networks.md) for membership.

## Host Firewall and Docker Publishing

UFW uses these default policies:

| Direction | Default policy |
| --- | --- |
| Incoming | Deny |
| Outgoing | Allow |
| Routed | Deny |

Selected infrastructure ports are explicitly permitted. Docker also maintains
its own nftables/iptables chains for published container ports.

Host firewall policy and Docker publishing interact. A published container port
must not be assumed blocked simply because it is absent from `ufw status`.
Assessing reachability requires considering the listener or published binding,
Docker's rules, host policy and upstream router/NAT configuration together.

## Exposure Boundary

Actual Internet exposure depends on upstream router/NAT forwarding together
with host listeners and Docker publishing. The diagrams show service
relationships, not externally reachable endpoints.

See [security](security.md) for SSH and enforcement limitations.
