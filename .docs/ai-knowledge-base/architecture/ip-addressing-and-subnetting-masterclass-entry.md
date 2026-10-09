# IP Addressing and Subnetting Masterclass — Knowledge Base Entry

## Role

This entry points to the authoritative IP addressing and subnetting series in the networking masterclass. It does not duplicate the technical analysis.

## Authoritative Document

- [Networking Masterclass Index](../../../networking/networking-masterclass/README.md)

## Chapter Links

- [IPv4 Addressing Fundamentals](../../../networking/networking-masterclass/02-ip-addressing-and-subnetting/01-ipv4-addressing-fundamentals.md)
- [CIDR and Subnet Masks](../../../networking/networking-masterclass/02-ip-addressing-and-subnetting/02-cidr-and-subnet-masks.md)
- [Subnetting by Design: VLSM and Route Summarization](../../../networking/networking-masterclass/02-ip-addressing-and-subnetting/03-subnetting-by-design-vlsm-and-summarization.md)
- [IPv6 Addressing](../../../networking/networking-masterclass/02-ip-addressing-and-subnetting/04-ipv6-addressing.md)

## Related Documents

- [Network Fundamentals Masterclass Entry](./network-fundamentals-masterclass-entry.md)
- [Cloud Virtual Networks Masterclass Entry](./cloud-virtual-networks-masterclass-entry.md)

## Decision Summary (Non-Substitutive)

IPv4 addresses are 32-bit numbers always interpreted with a prefix length; the mask defines the network/host boundary, and network and broadcast addresses are reserved, so usable host count is `2^(32-prefix) - 2` for normal prefixes. The historical classful system was inflexible and wasteful and was replaced by CIDR, which allows arbitrary prefixes and route aggregation. Address planning is a scarce-resource discipline: RFC 1918 private ranges are reused everywhere, so non-overlapping allocation (ideally via IPAM) is a prerequisite for connecting networks. VLSM conserves addresses by sizing each subnet to need, but requires disciplined bookkeeping and largest-first allocation to preserve contiguous space. Route summarization depends on contiguous, aligned allocations and keeps routing tables small. IPv6 changes the model: 128-bit addresses, a near-universal /64 on-link subnet convention, stateless autoconfiguration (SLAAC), link-local addresses on every interface, and no NAT by default—shifting the planning problem from conserving hosts to allocating enough /64s.

## Maintenance Note

When the addressing chapters evolve, update this entry only for link accuracy and the short decision summary. Keep mechanism-level explanation in the masterclass files.
