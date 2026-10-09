# Networking Masterclass Index

## Scope

This masterclass is a multi-document reference for senior-level networking, cloud networking, and transport-security architecture. It spans the full path from fundamentals (layering, Ethernet, IP, TCP/UDP, DNS) through IP addressing and subnetting, routing and switching, transport security (TLS/DTLS/mTLS), provider-agnostic cloud virtual networks, and operational observability and troubleshooting.

This module complements the database-focused track in the repository. It follows the same theory-heavy depth standard and cross-links where the two domains meet (for example, database connection encryption and service identity for data planes).

## Depth Standard

Each document in this module follows the repository's mandatory writing pattern:

- explicit purpose and decision context before implementation detail
- mechanism-level explanation of how things work, not only definitions
- invariants, assumptions, and boundary conditions
- trade-off analysis with rejected alternatives
- failure modes, degradation behavior, and recovery implications
- operational consequences (latency, availability, maintainability, cost, security posture)
- in-line glossary callouts for advanced terms
- worked examples and CLI labs where they build intuition
- visual support through Mermaid for complex flows and decision structures
- related-document cross-links and verified external references

## Document Map

### 01 Network Fundamentals

- [Models and Encapsulation](./01-network-fundamentals/01-models-and-encapsulation.md)
- [Layer 2: Ethernet, ARP, Switching, and VLANs](./01-network-fundamentals/02-layer-2-ethernet-arp-switching-and-vlans.md)
- [The IP Layer: Packet Delivery, TTL, MTU, and ICMP](./01-network-fundamentals/03-the-ip-layer-delivery-ttl-mtu-and-icmp.md)
- [The Transport Layer: TCP, UDP, Ports, and Sockets](./01-network-fundamentals/04-the-transport-layer-tcp-udp-ports-and-sockets.md)
- [Naming and Address Assignment: DNS, DHCP, and SLAAC](./01-network-fundamentals/05-naming-and-address-assignment-dns-dhcp-and-slaac.md)
- [NAT and Address Translation](./01-network-fundamentals/06-nat-and-address-translation.md)

### 02 IP Addressing and Subnetting

- [IPv4 Addressing Fundamentals](./02-ip-addressing-and-subnetting/01-ipv4-addressing-fundamentals.md)
- [CIDR and Subnet Masks](./02-ip-addressing-and-subnetting/02-cidr-and-subnet-masks.md)
- [Subnetting by Design: VLSM and Route Summarization](./02-ip-addressing-and-subnetting/03-subnetting-by-design-vlsm-and-summarization.md)
- [IPv6 Addressing](./02-ip-addressing-and-subnetting/04-ipv6-addressing.md)

### 03 Routing and Switching

- [Routing Fundamentals: Control Plane, Data Plane, and Longest-Prefix Match](./03-routing-and-switching/01-routing-fundamentals.md)
- [Interior Gateway Protocols: OSPF and Link-State/Distance-Vector Routing](./03-routing-and-switching/02-interior-gateway-protocols.md)
- [BGP and Internet Routing](./03-routing-and-switching/03-bgp-and-internet-routing.md)
- [Overlays and Tunnels: VXLAN, GRE, IPsec, and WireGuard](./03-routing-and-switching/04-overlays-and-tunnels.md)

### 04 Transport Security (SSL / TLS / mTLS / DTLS)

- [SSL to TLS: Evolution, Threat Model, and Security Guarantees](./04-transport-security/01-ssl-to-tls-evolution-and-threat-model.md)
- [The TLS Record Layer and the TLS 1.2 Handshake](./04-transport-security/02-record-layer-and-tls-1-2-handshake.md)
- [TLS 1.3: The Cryptographic Redesign](./04-transport-security/03-tls-1-3-cryptographic-redesign.md)
- [DTLS: TLS Over Datagram Transport](./04-transport-security/04-dtls-tls-over-datagram-transport.md)
- [PKI, X.509, and Certificate Trust Chains](./04-transport-security/05-pki-x509-and-certificate-trust-chains.md)
- [Key Exchange, Ciphers, and Forward Secrecy](./04-transport-security/06-key-exchange-ciphers-and-forward-secrecy.md)
- [Mutual TLS (mTLS): Identity, Zero Trust, and Service-to-Service Authentication](./04-transport-security/07-mutual-tls-identity-and-zero-trust.md)
- [TLS and DTLS in Production: Hardening, Debugging, and Failure Modes](./04-transport-security/08-tls-hardening-debugging-and-failure-modes.md)

### 05 Cloud Virtual Networks (Provider-Agnostic)

- [Cloud Virtual Networks: Fundamentals](./05-cloud-virtual-networks/01-virtual-network-fundamentals.md)
- [Subnets and Availability Zones](./05-cloud-virtual-networks/02-subnets-and-availability.md)
- [Gateways and Connectivity Primitives](./05-cloud-virtual-networks/03-gateways-and-connectivity-primitives.md)
- [Security Groups, Network ACLs, and Firewalls](./05-cloud-virtual-networks/04-security-groups-nacls-and-firewalls.md)
- [Connectivity Topologies: Peering, Transit, VPN, and Dedicated Links](./05-cloud-virtual-networks/05-connectivity-topologies.md)
- [Cloud DNS, Load Balancing, and Service Discovery](./05-cloud-virtual-networks/06-cloud-dns-load-balancing-and-service-discovery.md)
- [Multi-Account and Multi-Region Networking](./05-cloud-virtual-networks/07-multi-account-and-multi-region-networking.md)

### 06 Network Observability and Troubleshooting

- [Network Troubleshooting: Methodology and Tooling](./06-network-observability-and-troubleshooting/01-troubleshooting-methodology-and-tooling.md)
- [Network Performance and Capacity](./06-network-observability-and-troubleshooting/02-network-performance-and-capacity.md)

## Suggested Reading Order

The track is designed to be read in order for a first pass, because each section builds vocabulary and mechanisms that later sections assume:

1. **Network Fundamentals** — layer model, encapsulation, Ethernet/ARP, IP, TCP/UDP, DNS/DHCP, NAT. This is the vocabulary every later chapter uses.
2. **IP Addressing and Subnetting** — the arithmetic and design discipline for addressing and subnet design.
3. **Routing and Switching** — how packets are forwarded between networks and how overlays extend reach.
4. **Transport Security** — how data in transit is protected; builds directly on the transport layer.
5. **Cloud Virtual Networks** — how the same primitives are realized as provider-agnostic virtual networks.
6. **Network Observability and Troubleshooting** — how to measure and diagnose the whole stack.

For readers with a specific goal, the IP addressing and subnetting section and the transport-security section are the most self-contained, and the cloud section is intended for those designing multi-tenant or multi-region networks. Each chapter lists related documents for targeted follow-up.

## Visual Rule Compliance

Each document includes at least one Mermaid diagram representing a sequence flow, architecture relationship, state transition, or decision tree relevant to its mechanism.

## Position in the Repository Roadmap

- Previous/current phase: **Database and Distributed Data Systems**
- This phase: **Networking, Cloud Networking, and Transport Security**
- Next planned phases: **Linux Fundamentals**, **Cloud Architecture (compute/storage/identity)**, **Platform Reliability**

The intent is to build senior-level depth incrementally, one domain at a time, while preserving cross-links between topics.

## Related Knowledge Base Entries

- [Networking TLS Masterclass Entry](../../.docs/ai-knowledge-base/architecture/networking-tls-masterclass-entry.md)
- [Network Fundamentals Masterclass Entry](../../.docs/ai-knowledge-base/architecture/network-fundamentals-masterclass-entry.md)
- [IP Addressing and Subnetting Masterclass Entry](../../.docs/ai-knowledge-base/architecture/ip-addressing-and-subnetting-masterclass-entry.md)
- [Routing and Switching Masterclass Entry](../../.docs/ai-knowledge-base/architecture/routing-and-switching-masterclass-entry.md)
- [Cloud Virtual Networks Masterclass Entry](../../.docs/ai-knowledge-base/architecture/cloud-virtual-networks-masterclass-entry.md)
- [Network Observability Masterclass Entry](../../.docs/ai-knowledge-base/architecture/network-observability-masterclass-entry.md)
