# Networking Masterclass Index

## Scope

This masterclass is a multi-document reference for senior-level networking and transport-security architecture. Its first track covers the cryptographic protocols that protect data in transit: SSL/TLS, mutual TLS, and DTLS.

This module is a new phase of the broader repository journey, which currently focuses on databases and distributed data systems. It follows the same theory-heavy depth standard and cross-links to the database material wherever the two domains meet (for example, database connection encryption and service-identity for data planes).

## Depth Standard

Each document in this module follows the repository's mandatory writing pattern:

- explicit purpose and decision context before implementation detail
- mechanism-level explanation of how things work, not only definitions
- invariants, assumptions, and boundary conditions
- trade-off analysis with rejected alternatives
- failure modes, degradation behavior, and recovery implications
- operational consequences (latency, availability, maintainability, cost, security posture)
- in-line glossary callouts for advanced terms
- visual support through Mermaid for complex flows and decision structures

## Document Map

### 01 Transport Security (SSL / TLS / mTLS / DTLS)

- [SSL to TLS: Evolution, Threat Model, and Security Guarantees](./01-transport-security/01-ssl-to-tls-evolution-and-threat-model.md)
- [The TLS Record Layer and the TLS 1.2 Handshake](./01-transport-security/02-record-layer-and-tls-1-2-handshake.md)
- [TLS 1.3: The Cryptographic Redesign](./01-transport-security/03-tls-1-3-cryptographic-redesign.md)
- [DTLS: TLS Over Datagram Transport](./01-transport-security/04-dtls-tls-over-datagram-transport.md)
- [PKI, X.509, and Certificate Trust Chains](./01-transport-security/05-pki-x509-and-certificate-trust-chains.md)
- [Key Exchange, Ciphers, and Forward Secrecy](./01-transport-security/06-key-exchange-ciphers-and-forward-secrecy.md)
- [Mutual TLS (mTLS): Identity, Zero Trust, and Service-to-Service Authentication](./01-transport-security/07-mutual-tls-identity-and-zero-trust.md)
- [TLS and DTLS in Production: Hardening, Debugging, and Failure Modes](./01-transport-security/08-tls-hardening-debugging-and-failure-modes.md)

## Suggested Reading Order

The track is designed to be read in order for a first pass, because each chapter builds vocabulary and mechanisms that later chapters assume. The dependency shape is:

1. Start with the evolution and threat model chapter to fix the security goals and vocabulary.
2. Read the record layer and TLS 1.2 handshake chapter for the mechanical baseline.
3. Read TLS 1.3 to see what was removed and why.
4. Read DTLS to understand what changes when the transport is unreliable.
5. Read PKI, then key exchange and ciphers, to ground authentication and confidentiality in their real substructures.
6. Read mTLS to extend authentication to both directions.
7. Finish with the production chapter to convert theory into operations.

For readers who only need one topic, the PKI, mTLS, and production chapters are the most self-contained, but each references the earlier mechanism chapters for detail.

## Visual Rule Compliance

Each document includes at least one Mermaid diagram representing a sequence flow, architecture relationship, state transition, or decision tree relevant to its mechanism.

## Position in the Repository Roadmap

- Previous/current phase: **Database and Distributed Data Systems**
- This phase: **Networking and Transport Security**
- Next planned phases: **Linux Fundamentals**, **Cloud Architecture**, **Platform Reliability**

The intent is to build senior-level depth incrementally, one domain at a time, while preserving cross-links between topics.

## Related Knowledge Base Entries

- [Networking TLS Masterclass Entry](../../.docs/ai-knowledge-base/architecture/networking-tls-masterclass-entry.md)
