# Network Observability Masterclass — Knowledge Base Entry

## Role

This entry points to the authoritative network observability and troubleshooting series in the networking masterclass. It does not duplicate the technical analysis.

## Authoritative Document

- [Networking Masterclass Index](../../../networking/networking-masterclass/README.md)

## Chapter Links

- [Network Troubleshooting: Methodology and Tooling](../../../networking/networking-masterclass/06-network-observability-and-troubleshooting/01-troubleshooting-methodology-and-tooling.md)
- [Network Performance and Capacity](../../../networking/networking-masterclass/06-network-observability-and-troubleshooting/02-network-performance-and-capacity.md)

## Related Documents

- [Network Fundamentals Masterclass Entry](./network-fundamentals-masterclass-entry.md)
- [Cloud Virtual Networks Masterclass Entry](./cloud-virtual-networks-masterclass-entry.md)

## Decision Summary (Non-Substitutive)

Troubleshooting is a layered, hypothesis-driven discipline: because a lower-layer failure makes everything above it appear broken, diagnosis isolates the lowest failing layer (link, neighbor, addressing/routing, transport, DNS, TLS/application, policy) and uses observations that discriminate between hypotheses rather than guessing. Each tool proves something specific—`ip route get` for the forwarding decision, `ip neigh` for layer-2 resolution, `ping -M do` for PMTU, `ss -ti` for transport internals, `tcpdump` for wire truth, `conntrack` for translation state—and using the wrong tool produces false conclusions. Captures are authoritative but only as good as their vantage point, since NAT, tunnels, and hardware offload make the same packet look different at different points. A small set of symptom signatures (works by IP not by name; works small not large; one direction only; slow only under load) maps to a small set of causes (DNS, PMTU black hole, asymmetry/stateful filtering, congestion/bufferbloat). Performance decomposes into bandwidth, throughput, latency, jitter, and loss, and latency decomposes into propagation (physics-bound), serialization, queuing, and processing; throughput on high-latency paths is bounded by the bandwidth-delay product and window sizes, goodput is always below raw throughput, and bufferbloat inflates latency under load without visible loss. Measurement must target application-layer percentiles under realistic load, not idle averages.

## Maintenance Note

When the observability chapters evolve, update this entry only for link accuracy and the short decision summary. Keep mechanism-level explanation in the masterclass files.
