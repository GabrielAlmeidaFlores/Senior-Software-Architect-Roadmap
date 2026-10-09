# Cloud Virtual Networks Masterclass — Knowledge Base Entry

## Role

This entry points to the authoritative, provider-agnostic cloud virtual-networks series in the networking masterclass. It does not duplicate the technical analysis.

## Authoritative Document

- [Networking Masterclass Index](../../../networking/networking-masterclass/README.md)

## Chapter Links

- [Cloud Virtual Networks: Fundamentals](../../../networking/networking-masterclass/05-cloud-virtual-networks/01-virtual-network-fundamentals.md)
- [Subnets and Availability Zones](../../../networking/networking-masterclass/05-cloud-virtual-networks/02-subnets-and-availability.md)
- [Gateways and Connectivity Primitives](../../../networking/networking-masterclass/05-cloud-virtual-networks/03-gateways-and-connectivity-primitives.md)
- [Security Groups, Network ACLs, and Firewalls](../../../networking/networking-masterclass/05-cloud-virtual-networks/04-security-groups-nacls-and-firewalls.md)
- [Connectivity Topologies: Peering, Transit, VPN, and Dedicated Links](../../../networking/networking-masterclass/05-cloud-virtual-networks/05-connectivity-topologies.md)
- [Cloud DNS, Load Balancing, and Service Discovery](../../../networking/networking-masterclass/05-cloud-virtual-networks/06-cloud-dns-load-balancing-and-service-discovery.md)
- [Multi-Account and Multi-Region Networking](../../../networking/networking-masterclass/05-cloud-virtual-networks/07-multi-account-and-multi-region-networking.md)

## Related Documents

- [IP Addressing and Subnetting Masterclass Entry](./ip-addressing-and-subnetting-masterclass-entry.md)
- [Routing and Switching Masterclass Entry](./routing-and-switching-masterclass-entry.md)
- [Network Observability Masterclass Entry](./network-observability-masterclass-entry.md)

## Decision Summary (Non-Substitutive)

A cloud virtual network is a software-defined, isolated layer-3 domain with a customer-chosen address range, and isolation is enforced in software by the provider, not by physical separation—so design is a matter of declaring intent correctly, and address planning (non-overlapping, with headroom, ideally governed by IPAM) is the foundational decision. Subnets are CIDR segments bound to a failure domain (availability zone), and "public" versus "private" is a routing property determined by the subnet's route-table association, not a property of the subnet itself; cloud subnets reserve addresses, reducing usable capacity. Connectivity is provided by gateways and primitives: internet gateways (bidirectional, exposure surface), NAT gateways (outbound-only, often zonal, a cost center), egress-only gateways (IPv6 outbound-only), and private endpoints (private access to provider services avoiding the internet). Security is layered across stateful allow-only security groups (resource-scoped, referenceable by group), stateless ordered network ACLs (subnet-scoped, support deny, require return-path rules), and host firewalls. Topologies trade scalability and blast radius: non-transitive peering does not scale (quadratic), transit/hub gateways scale linearly but centralize risk, VPNs are fast but internet-dependent, and dedicated interconnects are predictable but costly. At scale, accounts are blast-radius containers and IPAM, centralized egress, cross-network DNS, and network-as-code become mandatory; regions are the highest failure domain, so multi-region design is required to survive region loss.

## Maintenance Note

When the cloud chapters evolve, update this entry only for link accuracy and the short decision summary. Keep mechanism-level explanation in the masterclass files.
