---
layout: default
title: "Glossary"
nav_order: 8
description: "PQC terms explained in plain engineering language"
---

# Glossary

Every term used in this guide, defined in plain engineering language.

---

### AES (Advanced Encryption Standard)
The standard symmetric encryption algorithm. Quantum computers weaken it (Grover's algorithm halves the effective key length), but AES-256 remains secure. **Why you care**: No migration needed — just use AES-256.

### Ciphertext
The encrypted output of an encryption or encapsulation operation. In KEM context, it's the data sent from sender to receiver that allows them to derive the shared secret. **Why you care**: PQC ciphertexts are larger than classical ones — budget for the bandwidth.

### Code-Based Cryptography
A PQC family based on error-correcting codes. Security comes from the difficulty of decoding a message with deliberately added errors without knowing the code structure. HQC uses this approach. **Why you care**: It's the backup plan if lattice math is ever broken — 45+ years of analysis with no breaks.

### Decapsulation (Decaps)
The KEM operation where the private key holder recovers the shared secret from a ciphertext. Analogous to the receiver's side of a Diffie-Hellman exchange. **Why you care**: This is where your server spends CPU during a TLS handshake.

### Digital Signature
A cryptographic proof that a message was created by a specific key holder and hasn't been tampered with. Used in TLS certificates, code signing, JWTs, and authentication. **Why you care**: PQC signatures (ML-DSA, SLH-DSA, FN-DSA) are much larger than ECDSA — impacts certificate chains and bandwidth.

### Encapsulation (Encaps)
The KEM operation where the sender generates a random shared secret and encrypts it using the receiver's public key. Produces a ciphertext that only the private key holder can decapsulate. **Why you care**: This is the client's side of a PQC key exchange.

### FIPS (Federal Information Processing Standards)
Standards published by NIST for cryptographic implementations. PQC standards: FIPS 203 (ML-KEM), 204 (ML-DSA), 205 (SLH-DSA), 206 (FN-DSA), 207 (HQC). **Why you care**: If you need government or compliance certification, you need FIPS-validated implementations.

### FN-DSA
FIPS 206, formerly FALCON. A lattice-based signature scheme using NTRU lattices. Produces the smallest PQC signatures (666 bytes) but requires floating-point arithmetic. **Why you care**: Use when bandwidth matters more than implementation simplicity.

### Forward Secrecy
A property ensuring that if long-term keys are compromised, past session keys remain secure. Achieved by using ephemeral key exchange (new keys per session). **Why you care**: PQC KEMs (ML-KEM) naturally provide forward secrecy when used with ephemeral keys in TLS.

### Grover's Algorithm
A quantum algorithm that speeds up brute-force search, effectively halving symmetric key lengths. AES-128 drops to 64-bit security; AES-256 drops to 128-bit (still secure). **Why you care**: Use AES-256 instead of AES-128. That's the only change needed for symmetric crypto.

### Hash-Based Cryptography
A PQC family that builds signatures using only hash functions (SHA-3, SHAKE). Minimal assumptions — if your hash function is secure, your signatures are secure. SLH-DSA uses this approach. **Why you care**: The most conservative PQC option, but with the largest signatures.

### Harvest-Now-Decrypt-Later (HNDL)
An attack strategy where adversaries record encrypted traffic today and store it until quantum computers can decrypt it. **Why you care**: If your data must stay secret for 10+ years, you're already within the threat window. Migrate key exchange to PQC now.

### HQC (Hamming Quasi-Cyclic)
FIPS 207 (selected March 2025). A code-based KEM providing algorithmic diversity as a backup to ML-KEM. Larger ciphertexts but based on a completely different mathematical family. **Why you care**: Use alongside ML-KEM in hybrid constructions for defense-in-depth against a lattice breakthrough.

### Hybrid Mode
Running classical and PQC algorithms in parallel during the transition period. The shared secret combines outputs from both: `secret = HKDF(classical || pqc)`. Secure if either algorithm is secure. **Why you care**: This is how you migrate safely — you don't need to trust PQC alone.

### KEM (Key Encapsulation Mechanism)
The PQC replacement for Diffie-Hellman key exchange. Instead of both parties computing a shared secret from their keys, one party encapsulates a random secret using the other's public key. The result is the same: a shared secret key. **Why you care**: Every TLS, SSH, and VPN key exchange will switch from DH/ECDH to a KEM.

### Lattice-Based Cryptography
The most widely used PQC family. Security comes from the difficulty of finding short vectors in high-dimensional mathematical structures called lattices. ML-KEM, ML-DSA, and FN-DSA all use this approach. **Why you care**: 3 of 5 NIST standards are lattice-based — it's the workhorse of PQC.

### LWE (Learning With Errors)
The core hard problem behind lattice-based PQC. Given a system of noisy linear equations, recover the secret values. Adding noise makes the problem exponentially hard, even for quantum computers. **Why you care**: This is what makes ML-KEM and ML-DSA secure. You don't need to understand the math — just know it's well-studied.

### ML-DSA
FIPS 204, formerly CRYSTALS-Dilithium. The default PQC signature algorithm. Lattice-based, with three parameter sets (ML-DSA-44/65/87). **Why you care**: This is your go-to replacement for RSA and ECDSA signatures.

### ML-KEM
FIPS 203, formerly CRYSTALS-Kyber. The default PQC key exchange algorithm. Lattice-based, with three parameter sets (ML-KEM-512/768/1024). **Why you care**: This is what replaces Diffie-Hellman and ECDH in TLS, SSH, and VPN.

### NIST Security Levels
A classification system for PQC algorithm strength:
- **Level 1**: Equivalent to AES-128 against quantum attack. Good for general use.
- **Level 3**: Equivalent to AES-192. Recommended default for most applications.
- **Level 5**: Equivalent to AES-256. For high-assurance, long-term secrets.

**Why you care**: Choose Level 3 unless you have specific constraints (bandwidth → Level 1, classified data → Level 5).

### Post-Quantum vs. Quantum-Safe
**Post-quantum** means resistant to attacks by both classical and quantum computers. **Quantum-safe** is used interchangeably. Neither term means "uses a quantum computer" — PQC runs on regular hardware. **Why you care**: Terminology matters in vendor evaluations. Both terms mean the same thing.

### Shor's Algorithm
A quantum algorithm that efficiently solves integer factoring and discrete logarithm problems — the mathematical foundations of RSA, DH, ECDH, and ECDSA. **Why you care**: This is THE reason PQC exists. Shor's algorithm breaks all classical public-key crypto.

### SLH-DSA
FIPS 205, formerly SPHINCS+. A hash-based signature scheme with the highest security confidence (only relies on hash functions). Large signatures (17-50 KB) but tiny public keys (32-64 bytes). **Why you care**: Use for high-assurance scenarios where you want defense against a lattice breakthrough.

### Syndrome Decoding
The hard problem behind code-based PQC (HQC). Given a syndrome (partial information about a codeword with errors), recover the original message. NP-hard even for quantum computers. **Why you care**: This is what makes HQC secure. The problem has been studied for 45+ years with no efficient quantum attack.

---

**Sources**: [NIST PQC FAQ](https://csrc.nist.gov/projects/post-quantum-cryptography/faqs)

*Last updated: 2026-02-13*
