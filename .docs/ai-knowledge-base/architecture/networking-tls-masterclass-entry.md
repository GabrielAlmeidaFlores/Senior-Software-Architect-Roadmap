# Networking and Transport Security (SSL / TLS / mTLS / DTLS) — Knowledge Base Entry

## Role

This entry points to the authoritative transport-security series in the networking masterclass. It does not duplicate the technical analysis.

## Authoritative Document

- [Networking Masterclass Index](../../../networking/networking-masterclass/README.md)

## Chapter Links

- [SSL to TLS: Evolution, Threat Model, and Security Guarantees](../../../networking/networking-masterclass/01-transport-security/01-ssl-to-tls-evolution-and-threat-model.md)
- [The TLS Record Layer and the TLS 1.2 Handshake](../../../networking/networking-masterclass/01-transport-security/02-record-layer-and-tls-1-2-handshake.md)
- [TLS 1.3: The Cryptographic Redesign](../../../networking/networking-masterclass/01-transport-security/03-tls-1-3-cryptographic-redesign.md)
- [DTLS: TLS Over Datagram Transport](../../../networking/networking-masterclass/01-transport-security/04-dtls-tls-over-datagram-transport.md)
- [PKI, X.509, and Certificate Trust Chains](../../../networking/networking-masterclass/01-transport-security/05-pki-x509-and-certificate-trust-chains.md)
- [Key Exchange, Ciphers, and Forward Secrecy](../../../networking/networking-masterclass/01-transport-security/06-key-exchange-ciphers-and-forward-secrecy.md)
- [Mutual TLS (mTLS): Identity, Zero Trust, and Service-to-Service Authentication](../../../networking/networking-masterclass/01-transport-security/07-mutual-tls-identity-and-zero-trust.md)
- [TLS and DTLS in Production: Hardening, Debugging, and Failure Modes](../../../networking/networking-masterclass/01-transport-security/08-tls-hardening-debugging-and-failure-modes.md)

## Related Documents

- [Database Sharding Masterclass Entry](./database-sharding-masterclass-entry.md)
- [Main SQL Databases Comparison Guide](../../../database/database-masterclass/02-relational-engines/05-main-sql-databases-comparison-guide.md)

## Decision Summary (Non-Substitutive)

Transport security is a threat-model decision, not a checkbox. TLS guarantees confidentiality, integrity, authentication, and (when ephemeral key exchange is used) forward secrecy against an active network adversary; it does not sanitize application data or protect endpoints. Modern posture converges on TLS 1.2 as an absolute floor with TLS 1.3 preferred, AEAD and ECDHE only, no CBC or RSA key transport, and named groups. DTLS adapts the same goals to unreliable datagrams by adding explicit epoch/sequence numbering, replay windows, flight retransmission, handshake fragmentation, stateless cookies, and connection identifiers. Authentication in practice is a PKI problem: failures cluster around expiry, incomplete chains, hostname/SAN mismatches, and trust-store divergence, and revocation is the ecosystem's weakest link, which is why short-lived certificates plus automation and Certificate Transparency are the prevailing direction. mTLS extends authentication to both directions and supplies workload identity for zero-trust and service-mesh architectures, at the cost of certificate lifecycle and trust-domain operations. Operationally, the highest-leverage controls are automated certificate lifecycle, enforced cipher/version policy on the server, client-perspective diagnostics, and continuous observability of negotiated parameters.

## Maintenance Note

When the masterclass chapters evolve, update this entry only for link accuracy and the short decision summary. Keep mechanism-level explanation in the masterclass files.
