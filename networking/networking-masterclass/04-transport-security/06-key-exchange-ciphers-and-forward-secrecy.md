# Key Exchange, Ciphers, and Forward Secrecy

## 1. Why This Chapter Exists

The handshake chapters described key exchange as a step that produces a shared secret, and the PKI chapter described how identities are authenticated. This chapter opens the cryptography underneath both: how two parties who have never met derive a shared secret over a channel an adversary can read, why some methods preserve forward secrecy and others do not, and how the resulting keys protect the actual data.

The goal is not to turn the reader into a cryptographer, but to make the trade-offs legible. Why is ephemeral Diffie-Hellman preferred to RSA key transport? Why is AES-GCM different from AES-CBC? Why does TLS 1.3 use HKDF? Why do some curves and key sizes appear in compliance lists? Architects who understand the mechanisms behind these questions can make defensible configuration decisions instead of copying cipher strings they cannot evaluate.

The chapter follows a deliberate progression: the two fundamental types of cryptography, the key-exchange problem and its solutions, the symmetric ciphers that protect bulk data, and finally the derivation and lifecycle of the keys that tie it together. Each part connects to the forward-secrecy property introduced in the first chapter.

---

## 2. Symmetric and Asymmetric Cryptography

**Symmetric cryptography** uses the same secret key to encrypt and decrypt. It is fast—modern CPUs can process gigabytes per second with hardware instructions—and is therefore the right tool for bulk data. Its weakness is key distribution: if two parties want to communicate secretly, they must already share a key, and getting that shared key to both of them securely is a chicken-and-egg problem.

**Asymmetric (public-key) cryptography** uses a mathematically linked key pair: a public key that can be shared freely and a private key kept secret. Operations include encryption (anyone can encrypt to the public key; only the private key decrypts), signatures (only the private key signs; anyone with the public key verifies), and key agreement (the two key pairs negotiate a shared secret without transmitting it). Asymmetric operations are orders of magnitude slower than symmetric ones and cannot handle bulk data.

The design of TLS follows directly from these properties. TLS uses **asymmetric cryptography only during the handshake**, to authenticate identity and to establish a symmetric secret, and then uses **symmetric cryptography for all application data**. This hybrid approach gets the security properties of public-key cryptography and the performance of symmetric cryptography. The only question that remains is how the handshake establishes the symmetric key.

---

## 3. The Key Exchange Problem

The problem: two parties want to agree on a secret, over a channel the adversary can fully observe, without having previously shared anything. Asymmetric cryptography provides two families of solutions, and their difference is the heart of forward secrecy.

### 3.1 RSA Key Transport (No Forward Secrecy)

In RSA key transport, the client invents a random **premaster secret**, encrypts it with the server's RSA public key (from the certificate), and sends the ciphertext. The server decrypts with its private key. Both now share the secret.

The critical weakness is that **the security of every session depends entirely on the server's long-term private key**. An adversary who records the ciphertext today and later obtains the server's private key—through compromise, legal compulsion, or cryptanalysis—can decrypt the recorded premaster secret and thus the entire session. There is no forward secrecy. This is not a bug but a structural property of encrypting the secret *to* a long-lived key. RSA key transport was removed in TLS 1.3.

### 3.2 Diffie-Hellman Key Agreement (Forward Secret)

Diffie-Hellman (DH) is a key **agreement** protocol rather than a key transport. In its simplest form, two parties each choose a private exponent, compute a public value, exchange public values, and each raises the other's public value to their own private exponent. Modular arithmetic guarantees both arrive at the same result: `g^(ab)`, where `a` and `b` are the private exponents. An observer sees `g^a` and `g^b` but cannot efficiently compute `g^(ab)` without solving the **Diffie-Hellman problem**, which is believed hard for well-chosen groups.

The forward-secrecy property comes from **ephemerality**. In **Ephemeral Diffie-Hellman (DHE)** or **Elliptic-Curve Ephemeral Diffie-Hellman (ECDHE)**, the private exponents are generated fresh for each handshake and destroyed afterward. The server authenticates its public value by signing it with its long-term key, but the session secret itself is never derived from the long-term key. Consequently, an adversary who later steals the long-term key can forge future impersonations but cannot recover past session keys, because those depended on ephemeral values that no longer exist.

```mermaid
sequenceDiagram
    participant C as Client
    participant S as Server
    Note over C: pick a, compute g^a
    Note over S: pick b, compute g^b
    C->>S: g^a
    S->>C: g^b, signature over (g^b)
    Note over C: verify signature, compute (g^b)^a = g^ab
    Note over S: compute (g^a)^b = g^ab
    Note over C,S: shared secret g^ab; a and b discarded
```

### 3.3 Elliptic Curves and Efficiency

Modern deployments overwhelmingly use **ECDHE** rather than finite-field DHE. Elliptic-curve cryptography achieves equivalent security with much smaller keys: a 256-bit EC key provides roughly the security of a 3072-bit RSA key or a large finite-field DH group. Smaller keys mean faster computation and smaller handshakes, which matters for latency and for constrained devices. TLS 1.3 restricts negotiation to named, vetted curves and groups (for example X25519 and the NIST P-curves), removing the risk of weak custom parameters that enabled attacks such as Logjam.

### 3.4 Why the Signature Is Essential

Ephemeral Diffie-Hellman alone is vulnerable to a man-in-the-middle: an active attacker could perform two separate DH exchanges, one with each party, and sit in the middle. TLS prevents this by having the server **sign** its ephemeral public value with the private key corresponding to its certificate. The client verifies the signature against the certificate's public key, which is trusted via the PKI chain. This is the exact meeting point of the PKI chapter and this one: the signature binds the ephemeral key exchange to the verified identity. Without the signature, DH gives confidentiality against a passive observer but not authentication against an active one.

#### In-Line Glossary: Diffie-Hellman Problem

**What it is:** given `g^a` and `g^b`, computing `g^(ab)` is believed computationally infeasible for appropriate groups.

**Why here:** it is the hardness assumption that makes DH key agreement secure against a network observer who sees the public values.

**Systemic implication:** if the group is weak or small (as in export-grade DH), the problem becomes tractable and the session secret is recoverable, which is exactly the Logjam class of attack.

---

## 4. Symmetric Ciphers: Block, Stream, and AEAD

Once a shared secret exists, it becomes a **symmetric session key** that encrypts application data in the record layer. The choice of symmetric construction has profound practical consequences.

### 4.1 Block Ciphers and Modes of Operation

A **block cipher** such as AES encrypts fixed-size blocks (128 bits for AES). Encrypting more than one block requires a **mode of operation**. **ECB** (Electronic Codebook) encrypts each block independently and is dangerously wrong for real data because identical plaintext blocks produce identical ciphertext blocks, leaking structure. **CBC** (Cipher Block Chaining) XORs each plaintext block with the previous ciphertext block, so identical plaintexts differ, but as the TLS history showed, CBC combined with MAC-then-Encrypt and observable error behavior leaks through padding oracles. **CTR** (Counter mode) turns a block cipher into a stream cipher by encrypting sequential counter values and XORing with the plaintext; it is fast and parallelizable but provides no integrity on its own, so it must be paired with a MAC (as in encrypt-then-MAC) or used inside an AEAD construction.

### 4.2 AEAD: Confidentiality and Integrity Together

**AEAD (Authenticated Encryption with Associated Data)** inverts the typical mistake of building encryption from separately composed primitives and instead provides confidentiality and integrity in one well-analyzed operation. The two dominant AEAD ciphers in TLS are **AES-GCM** and **ChaCha20-Poly1305**.

**AES-GCM** combines AES in counter mode with the Galois Message Authentication Code. On CPUs with AES-NI hardware instructions it is extremely fast. Its main caveat is that **nonce reuse is catastrophic**: encrypting two different messages under the same key and nonce can allow an attacker to recover the authentication key and forge arbitrary messages. TLS avoids this by deriving the nonce deterministically from the record sequence number and the traffic secret, so the same nonce is never reused under the same key. This is a design decision that shifts a dangerous requirement from the application to the protocol.

**ChaCha20-Poly1305** combines the ChaCha20 stream cipher with the Poly1305 authenticator. It is designed to be fast in **software**, without special hardware, which makes it the preferred choice on CPUs lacking AES acceleration (many mobile and embedded processors). It is also constant-time by construction in ways that are easier to implement safely in software. The practical rule is to offer both and let the client and server select based on what each does best.

#### In-Line Glossary: Nonce Reuse

**What it is:** encrypting two distinct messages under the same key and the same nonce.

**Why here:** for GCM it collapses both confidentiality and integrity, potentially exposing the authentication key.

**Systemic implication:** the protocol's deterministic nonce derivation is a hard requirement, and custom or streaming protocols using GCM must treat nonce management as security-critical.

### 4.3 Why AEAD Replaced the Old Constructions

The shift to AEAD is not a preference but a correction. MAC-then-Encrypt created the padding-oracle class. Separately composed encrypt-and-MAC schemes are easy to get subtly wrong. AEAD's single operation with a well-studied proof removes both the composition hazard and the padding oracle, because there is no padding to probe and decryption either succeeds with a valid tag or fails uniformly. This is why TLS 1.3 permits only AEAD.

#### In-Line Glossary: AEAD

**What it is:** an encryption construction that provides confidentiality and integrity in a single primitive, optionally authenticating associated data that is not encrypted.

**Why here:** it removes the ordering mistakes and padding oracles of composing a cipher with a separate MAC, which is why TLS 1.3 makes it the only permitted mode.

**Systemic implication:** AEAD shifts a class of subtle implementation risk out of application and protocol code and into a small, well-analyzed primitive, at the cost of imposing strict nonce-uniqueness requirements.

---

## 5. Key Derivation and Key Separation

A shared secret is not directly usable as an encryption key. It must be expanded into multiple independent keys: one for each direction of traffic, plus nonces, plus material for integrity and for application export. This expansion is **key derivation**, and doing it carelessly is a classic source of vulnerabilities.

TLS 1.2 used a custom PRF built from HMAC (typically HMAC-SHA256) to expand the premaster secret into the master secret and then the key block. TLS 1.3 formalized this into the HKDF-based key schedule described in the 1.3 chapter. The essential property in both is **key separation through labeling**: different keys are derived with distinct labels or contexts so that knowledge of one key reveals nothing about another. If client and server keys were the same, or if encryption and integrity used the same key material, one weakness could cascade into others.

Another essential property is **uniqueness per session**. Both the client and server contribute random nonces (the client random and server random), and these are mixed into the derivation. This ensures that even if two sessions somehow negotiated the same premaster secret—an unlikely but catastrophic event if it happened—the derived keys would still differ because the randoms differ.

---

## 6. Forward Secrecy: The Complete Picture

We can now state forward secrecy precisely and see why it is a property of the key exchange, not of the cipher.

Forward secrecy holds when the session key is derived from information that is **destroyed after the handshake** and is **not recoverable from any long-lived secret**. Ephemeral Diffie-Hellman satisfies this because the ephemeral private exponents are generated per handshake and discarded. RSA key transport fails it because the session key is encrypted to a long-lived key. Static Diffie-Hellman would fail it because the same long-term DH key is reused across sessions, so compromise of that key compromises all of them.

It is worth being precise about the residual risks even with forward secrecy. Forward secrecy protects the **confidentiality of past session keys**; it does not protect against an active attacker who compromises the long-lived key and then impersonates the server for **future** sessions. It also does not protect **0-RTT** early data in the way it protects full handshakes, for the reasons given in the 1.3 chapter. And it depends on the ephemeral randomness being genuinely random; a predictable RNG can make ephemeral keys guessable, collapsing the property. These boundaries are why "we have forward secrecy" is a statement about a specific threat, not a blanket immunity.

```mermaid
flowchart TD
    A["Long-term key compromised AFTER session"] --> B{"Key exchange type?"}
    B -->|RSA key transport| C["Session decrypted: NO forward secrecy"]
    B -->|Static DH| D["Session decrypted: NO forward secrecy"]
    B -->|Ephemeral DH (DHE/ECDHE)| E["Session safe: forward secrecy holds"]
    E --> F["Provide RNG was good and keys were destroyed"]
```

---

## 7. Invariants, Assumptions, and Boundaries

**Invariant: the session key is never transmitted in clear.** It is either encrypted to a long-term key (RSA transport) or computed independently by both sides (DH). The network never sees it.

**Invariant: the ephemeral key exchange is authenticated by a signature over the transcript.** This is what defeats the man-in-the-middle for DH.

**Invariant: keys are separated by direction and purpose.** Client and server write keys differ, and encryption material is distinct from integrity and exporter material.

**Invariant: nonces are unique per key.** TLS derives them deterministically from sequence numbers to prevent reuse.

**Assumption: the underlying hard problems are hard.** RSA, finite-field DH, and elliptic-curve discrete log are assumed infeasible to solve at deployed key sizes. Shor's algorithm on a sufficiently large quantum computer would break all of them, which is the motivation for post-quantum key exchange research and hybrid deployment.

**Assumption: random number generation is sound.** Every ephemeral secret and nonce depends on it.

**Boundary: symmetric encryption protects data in transit, not the endpoints.** Key material resident in process memory can leak through core dumps, side channels, or memory-safety bugs.

**Boundary: computational hardness is a moving target.** "Secure for now" is the honest standard; algorithms and key sizes are deprecated over decades as attacks and computing power improve.

---

## 8. Trade-offs and Rejected Alternatives

**Why prefer ECDHE over finite-field DHE?** Smaller keys, faster operations, smaller handshakes, and a narrower set of well-vetted parameters. Finite-field DHE with adequate key sizes is still secure but bulkier and more error-prone to configure. The rejected alternative, staying on finite-field DH for compatibility, sacrifices performance and parameter safety for a smaller and shrinking compatibility gain.

**Why not use a single modern cipher for everything?** Because different CPUs are good at different things. AES-GCM with hardware acceleration beats ChaCha20 on server CPUs with AES-NI, while ChaCha20 beats AES-GCM on devices without it and is easier to implement in constant time in software. Offering both and selecting per client is strictly better than forcing one. The rejected alternative, mandating a single suite, either penalizes hardware-accelerated servers or software-only clients.

**Why not use longer keys for more security?** Because security is bounded by the weakest link and by the hard-problem's actual difficulty, not by key size alone. Oversized keys cost latency and CPU in the handshake with no meaningful gain once the security margin is sufficient (for example, X25519's 128-bit security level). The rejected alternative, maximal key sizes everywhere, is performance theater.

**Why keep forward secrecy optional anywhere?** It is not; TLS 1.3 removed the option, and TLS 1.2 deployments should configure it. Where static keys persist, it is almost always an unexamined default, not a deliberate choice.

---

## 9. Failure Modes and Operational Consequences

**Weak or export-grade parameters.** Old configurations that permit small DH groups or short RSA keys expose sessions to recovery. Operational consequence: restrict to named groups and modern key sizes; audit for any legacy suite that re-enables weak parameters.

**Nonce reuse through misconfiguration or custom code.** GCM with a fixed nonce destroys security silently. Operational consequence: never hand-roll record encryption; rely on TLS libraries that derive nonces correctly, and treat any custom GCM usage as a red flag.

**Weak randomness.** A faulty RNG (as in the historical Debian OpenSSL bug) makes ephemeral keys and nonces predictable, collapsing key exchange and forward secrecy. Operational consequence: use well-vetted system randomness, keep libraries patched, and be especially cautious in virtualized and early-boot environments where entropy may be scarce.

**CBC retained for compatibility.** Configurations that still offer CBC suites carry padding-oracle risk. Operational consequence: prefer AEAD-only suites; if CBC is unavoidable, ensure constant-time implementations and uniform error handling.

**CPU cost at scale.** The asymmetric operations in a full handshake are the dominant CPU cost per connection. At high connection churn, this becomes a capacity concern and drives session resumption and hardware acceleration. Operational consequence: size handshake capacity explicitly, enable resumption, and use hardware crypto where available.

**Quantum exposure.** Traffic encrypted today with classical key exchange can be recorded and, in principle, decrypted later by a future quantum adversary (harvest-now, decrypt-later). Operational consequence: track post-quantum and hybrid key exchange standards and plan migration for long-lived confidentiality requirements, even though the risk is not immediate.

---

## 10. External References

- [RFC 8446: The Transport Layer Security (TLS) Protocol Version 1.3](https://www.rfc-editor.org/rfc/rfc8446)
- [RFC 5869: HMAC-based Extract-and-Expand Key Derivation Function (HKDF)](https://www.rfc-editor.org/rfc/rfc5869)
- [RFC 8439: ChaCha20 and Poly1305 for IETF Protocols](https://www.rfc-editor.org/rfc/rfc8439)
- [RFC 8422: Elliptic Curve Cryptography (ECC) Cipher Suites for TLS](https://www.rfc-editor.org/rfc/rfc8422)
- [RFC 7919: Negotiated Finite Field Diffie-Hellman Ephemeral Parameters for TLS](https://www.rfc-editor.org/rfc/rfc7919)
- [RFC 7525: Recommendations for Secure Use of TLS and DTLS (BCP 195)](https://www.rfc-editor.org/rfc/rfc7525)
- [TLS 1.3: The Cryptographic Redesign](./03-tls-1-3-cryptographic-redesign.md)
