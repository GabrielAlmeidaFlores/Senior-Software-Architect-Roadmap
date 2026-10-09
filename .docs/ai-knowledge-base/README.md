# AI Knowledge Base

This directory is reserved for AI-maintained project knowledge in Markdown format.

## Scope

- Architecture notes
- ADR references
- Operational runbooks
- Incident learnings
- Design trade-off analyses
- Glossaries and conceptual deep dives

## Architecture Knowledge

- [Database Sharding Masterclass Entry](./architecture/database-sharding-masterclass-entry.md) records the authoritative learning document, its source context, and related material without duplicating the technical analysis.
- [PostgreSQL vs MySQL Comparison Entry](./architecture/postgres-vs-mysql-comparison-entry.md) points to the masterclass comparison of MVCC cleanup, locking, and operational failure modes without duplicating the technical analysis.
- [PostgreSQL Deep Dive Entry](./architecture/postgresql-deep-dive-entry.md) points to the PostgreSQL process model, MVCC/vacuum, HOT, wraparound, and WAL teaching chapter.
- [MySQL InnoDB Architecture Entry](./architecture/mysql-innodb-architecture-entry.md) points to the InnoDB buffer, locking, undo/purge, and replication teaching chapter.
- [Index Internals Masterclass Entry](./architecture/index-internals-masterclass-entry.md) points to the B-tree versus LSM amplification and index failure-mode teaching chapter.
- [Networking TLS Masterclass Entry](./architecture/networking-tls-masterclass-entry.md) points to the SSL/TLS, TLS 1.3, DTLS, PKI, forward-secrecy, mTLS, and production-hardening teaching series without duplicating the technical analysis.
- [Network Fundamentals Masterclass Entry](./architecture/network-fundamentals-masterclass-entry.md) points to the layering/encapsulation, Ethernet/ARP/VLAN, IP/ICMP, TCP/UDP, DNS/DHCP/SLAAC, and NAT teaching series without duplicating the technical analysis.
- [IP Addressing and Subnetting Masterclass Entry](./architecture/ip-addressing-and-subnetting-masterclass-entry.md) points to the IPv4 fundamentals, CIDR/masks, VLSM/summarization, and IPv6 addressing teaching series without duplicating the technical analysis.
- [Routing and Switching Masterclass Entry](./architecture/routing-and-switching-masterclass-entry.md) points to the routing fundamentals, OSPF/IGP, BGP, and overlay/tunnel teaching series without duplicating the technical analysis.
- [Cloud Virtual Networks Masterclass Entry](./architecture/cloud-virtual-networks-masterclass-entry.md) points to the provider-agnostic virtual network, subnet, gateway, security, topology, DNS/LB, and multi-account/multi-region teaching series without duplicating the technical analysis.
- [Network Observability Masterclass Entry](./architecture/network-observability-masterclass-entry.md) points to the troubleshooting methodology and network performance/capacity teaching series without duplicating the technical analysis.

## Maintenance Rules

- Keep all content in English.
- Prefer focused documents and cross-link related topics.
- Update existing documents when decisions evolve.

## Mandatory Writing Pattern (Applies to All Future Domains)

Every AI-authored document in this knowledge base must follow a deep, explicit, theory-heavy pattern:

- Explain the purpose and context before implementation details.
- Break down internal mechanisms step by step.
- State assumptions, invariants, and boundaries clearly.
- Include trade-off analysis and alternative choices.
- Cover failure modes, resilience expectations, and recovery behavior.
- Connect technical choices to operational impact (latency, consistency, availability, cost, maintainability).
- Add in-line glossary sections for advanced concepts.
- Use Mermaid diagrams for complex flows and architecture relationships when helpful.
- Use external references when they materially improve understanding.

### No Shallow Noun-Only Bullet Lists

Do not leave concepts as unexplained labels such as:

- referential integrity
- uniqueness constraints
- ACID-C is invariant preservation

Instead, write complete explanatory text for each idea covering:

- what the concept is
- how it works in this context
- why it matters
- what happens if it is ignored

Bullet lists are allowed only when each bullet is already a full explanatory sentence or short paragraph. Prefer paragraphs over noun-phrase lists for examples, trade-offs, benefits, costs, and decision criteria.

Depth should favor clarity over brevity. Long-form explanations are preferred when they improve reader understanding.
