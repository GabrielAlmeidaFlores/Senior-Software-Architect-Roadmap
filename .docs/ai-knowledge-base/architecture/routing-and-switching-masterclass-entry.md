# Routing and Switching Masterclass — Knowledge Base Entry

## Role

This entry points to the authoritative routing and switching series in the networking masterclass. It does not duplicate the technical analysis.

## Authoritative Document

- [Networking Masterclass Index](../../../networking/networking-masterclass/README.md)

## Chapter Links

- [Routing Fundamentals: Control Plane, Data Plane, and Longest-Prefix Match](../../../networking/networking-masterclass/03-routing-and-switching/01-routing-fundamentals.md)
- [Interior Gateway Protocols: OSPF and Link-State/Distance-Vector Routing](../../../networking/networking-masterclass/03-routing-and-switching/02-interior-gateway-protocols.md)
- [BGP and Internet Routing](../../../networking/networking-masterclass/03-routing-and-switching/03-bgp-and-internet-routing.md)
- [Overlays and Tunnels: VXLAN, GRE, IPsec, and WireGuard](../../../networking/networking-masterclass/03-routing-and-switching/04-overlays-and-tunnels.md)

## Related Documents

- [IP Addressing and Subnetting Masterclass Entry](./ip-addressing-and-subnetting-masterclass-entry.md)
- [Cloud Virtual Networks Masterclass Entry](./cloud-virtual-networks-masterclass-entry.md)
- [Networking TLS Masterclass Entry](./networking-tls-masterclass-entry.md)

## Decision Summary (Non-Substitutive)

Routing separates a control plane (protocols and static config producing the RIB) from a data plane (per-packet longest-prefix-match forwarding via the FIB); mismatches between the two produce behavior that contradicts configuration. Longest-prefix match makes routing hierarchical and summarizable and is why a stale specific route or an over-broad summary silently breaks traffic regardless of metrics. Best-path selection is two-stage: administrative distance chooses the most trustworthy source, then metrics choose the path within that source; metrics are protocol-local. Interior routing splits into distance-vector (simple, slow-converging, loop-prone) and link-state (flooding plus SPF, fast and loop-free, at higher resource cost), with OSPF areas bounding cost and redistribution being a frequent source of leaks and loops. At internet scale, BGP is a policy-first path-vector protocol whose decisions encode commercial relationships; it has no built-in origin authentication, so hijacks and leaks occur and are mitigated by filtering and RPKI. Overlays (VXLAN, GRE, IPsec, WireGuard) build logical topologies over an underlay, gaining flexibility and isolation at the cost of encapsulation overhead—most visibly the MTU overhead that causes size-dependent failures.

## Maintenance Note

When the routing chapters evolve, update this entry only for link accuracy and the short decision summary. Keep mechanism-level explanation in the masterclass files.
