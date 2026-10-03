# Networking

Networking combines a private LAN, an upstream router, Docker networks and
infrastructure services for DNS, reverse proxying and VPN access. Public
addresses, domains, peer details and externally reachable ports are omitted.

## Connectivity Roles

| Component | Role |
| --- | --- |
| AdGuard Home | DNS service, using host networking |
| Nginx Proxy Manager | Reverse proxy service on `nginx-proxy` |
| WireGuard / wg-easy | VPN access on its project bridge |
| OpenVPN | VPN access on Docker's default bridge |
| Upstream router / NAT | Determines routing and forwarding toward the host |

Specific client DNS assignments, proxy routes, certificates and VPN routing
policies are not reproduced. VPN deployment does not by itself demonstrate a
recent successful connection from every client.

## Service-Oriented Docker Separation

Reverse proxy, database and monitoring components use separate networks.
This organizes connectivity around service relationships rather than placing
every container on one shared bridge.

n8n joins both `nginx-proxy` and `database` to reach the components it needs.
This is intentional multi-network membership, not an accidental duplicate
deployment. Other projects have their own networks.

AdGuard Home and Netdata share the host network namespace. Their listeners
must be considered as host-network listeners rather than bridge-published ports.
OpenVPN and WireGuard use different bridge arrangements.

See the [Docker network diagram](../diagrams/docker-networks.md) for membership.
Network separation is not presented as zero-trust or comprehensive isolation.

## Host Firewall and Docker Publishing

The verified UFW policy is approximately:

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

This documentation describes service relationships, not the exact external
topology. A reverse proxy or VPN container being deployed is not evidence that
its port is reachable from the Internet. Current forwarding and reachable
endpoints require a separate owner review before making exposure claims.

See [security](security.md) for SSH and enforcement limitations.
