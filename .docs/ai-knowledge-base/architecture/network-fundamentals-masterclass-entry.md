# Network Fundamentals Masterclass — Knowledge Base Entry

## Role

This entry points to the authoritative network-fundamentals series in the networking masterclass. It does not duplicate the technical analysis.

## Authoritative Document

- [Network Fundamentals Index](../../../networking/networking-masterclass/README.md)

## Chapter Links

- [Models and Encapsulation](../../../networking/networking-masterclass/01-network-fundamentals/01-models-and-encapsulation.md)
- [Layer 2: Ethernet, ARP, Switching, and VLANs](../../../networking/networking-masterclass/01-network-fundamentals/02-layer-2-ethernet-arp-switching-and-vlans.md)
- [The IP Layer: Packet Delivery, TTL, MTU, and ICMP](../../../networking/networking-masterclass/01-network-fundamentals/03-the-ip-layer-delivery-ttl-mtu-and-icmp.md)
- [The Transport Layer: TCP, UDP, Ports, and Sockets](../../../networking/networking-masterclass/01-network-fundamentals/04-the-transport-layer-tcp-udp-ports-and-sockets.md)
- [Naming and Address Assignment: DNS, DHCP, and SLAAC](../../../networking/networking-masterclass/01-network-fundamentals/05-naming-and-address-assignment-dns-dhcp-and-slaac.md)
- [NAT and Address Translation](../../../networking/networking-masterclass/01-network-fundamentals/06-nat-and-address-translation.md)

## Related Documents

- [IP Addressing and Subnetting Masterclass Entry](./ip-addressing-and-subnetting-masterclass-entry.md)
- [Networking TLS Masterclass Entry](./networking-tls-masterclass-entry.md)

## Decision Summary (Non-Substitutive)

Networks are layered stacks in which each layer adds a header and solves one problem, and each layer only inspects its own header; this opacity is what makes the stack composable and what makes failures diagnosable only at the correct layer. The IP layer is deliberately minimal and best-effort, which pushes reliability and ordering to TCP and forces applications to handle what IP does not promise. TCP provides a reliable, ordered byte stream with flow control (receive window) and congestion control (send window), while UDP provides independent datagrams without guarantees—making reliability an application concern. MTU and encapsulation overhead are recurring sources of size-dependent failures (PMTU black holes), and congestion control makes throughput a function of RTT, loss, and window size. DNS resolution, DHCP/SLAAC address assignment, and NAT translation all precede or rewrite application traffic and account for a large share of production incidents. The operational through-line is that TLS/DTLS builds directly on this transport layer, and that most "network is down" reports are actually failures of DNS, address assignment, MTU, or stateful filtering rather than raw connectivity.

## Maintenance Note

When the fundamentals chapters evolve, update this entry only for link accuracy and the short decision summary. Keep mechanism-level explanation in the masterclass files.
