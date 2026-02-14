---
layout: default
title: "Why PQC Matters"
nav_order: 2
description: "The quantum threat explained for engineers"
---

# Why PQC Matters

Every HTTPS connection, SSH session, and VPN tunnel you operate relies on public-key cryptography — RSA, ECDSA, Diffie-Hellman, ECDH. A sufficiently powerful quantum computer breaks **all of them**.

This isn't theoretical hand-wringing. NIST has published replacement standards and set a **2035 deadline** to deprecate classical algorithms.

## What Breaks and Why

In 1994, mathematician Peter Shor discovered a quantum algorithm that efficiently factors large integers and computes discrete logarithms — the two hard problems that all modern public-key crypto depends on.

**The short version**: Shor's algorithm turns exponentially hard problems into polynomial-time problems on a quantum computer. RSA's security comes from "multiplying two primes is easy, factoring the product is hard." A quantum computer makes factoring easy too.

```mermaid
flowchart TD
    SHOR[Shor's Algorithm] --> RSA[RSA — Broken]
    SHOR --> DH[Diffie-Hellman — Broken]
    SHOR --> ECDSA[ECDSA — Broken]
    SHOR --> ECDH[ECDH — Broken]
    SHOR --> DSA[DSA — Broken]

    GROVER[Grover's Algorithm] --> AES[AES — Weakened]
    GROVER --> SHA[SHA — Weakened]

    AES --> FIX1[Fix: Use AES-256<br/>instead of AES-128]
    SHA --> FIX2[Fix: Use SHA-384/512<br/>or double output size]

    classDef broken fill:#8e5050,stroke:#6e3030,color:#2c3e50
    classDef weakened fill:#8e7060,stroke:#6e5040,color:#2c3e50
    classDef fix fill:#6b8e6b,stroke:#4a6a4a,color:#2c3e50
    classDef algo fill:#5a8eaa,stroke:#3a6e8a,color:#2c3e50

    class SHOR,GROVER algo
    class RSA,DH,ECDSA,ECDH,DSA broken
    class AES,SHA weakened
    class FIX1,FIX2 fix
```

| What | Status | Impact |
|------|--------|--------|
| RSA (all key sizes) | Completely broken | TLS, code signing, certificates |
| ECDSA / EdDSA | Completely broken | TLS, SSH, JWT, blockchain |
| Diffie-Hellman / ECDH | Completely broken | Key exchange in TLS, VPN, SSH |
| AES-128 | Weakened to 64-bit | Use AES-256 instead |
| AES-256 | Safe (128-bit quantum) | No change needed |
| SHA-256 | Weakened to 128-bit | Still adequate for most uses |

**Key takeaway**: All **public-key** cryptography is broken. **Symmetric** cryptography (AES, SHA) survives with larger key/output sizes.

## The Harvest-Now-Decrypt-Later Threat

This is why migration is urgent **today**, even before large quantum computers exist.

Adversaries — nation-states in particular — are recording encrypted traffic **right now**. When quantum computers become available, they'll decrypt everything they've stored.

```mermaid
flowchart LR
    A[Adversary records<br/>encrypted traffic<br/>TODAY] --> B[Stores<br/>ciphertext<br/>for years]
    B --> C[Quantum computer<br/>becomes available<br/>2030s?]
    C --> D[Decrypts all<br/>stored data]

    classDef dark fill:#5a8eaa,stroke:#3a6e8a,color:#2c3e50
    classDef broken fill:#8e5050,stroke:#6e3030,color:#2c3e50

    class A,B,C dark
    class D broken
```

If your data needs to stay confidential for **more than 10 years** (medical records, financial data, government communications, trade secrets), the threat window is **already open**.

## The Timeline

```mermaid
flowchart LR
    S1[1994<br/>Shor's algorithm<br/>published] --> S2[2016<br/>NIST PQC<br/>competition<br/>launched]
    S2 --> S3[2024<br/>FIPS 203-205<br/>published]
    S3 --> S4[2025<br/>FIPS 206-207<br/>expected]
    S4 --> S5[2030<br/>Recommended<br/>migration<br/>complete]
    S5 --> S6[2035<br/>Classical crypto<br/>DEPRECATED]

    classDef past fill:#6b8e6b,stroke:#4a6a4a,color:#2c3e50
    classDef now fill:#8e6b8e,stroke:#6e4a6e,color:#2c3e50
    classDef future fill:#8e7060,stroke:#6e5040,color:#2c3e50
    classDef deadline fill:#8e5050,stroke:#6e3030,color:#2c3e50

    class S1,S2 past
    class S3,S4 now
    class S5 future
    class S6 deadline
```

| Date | Event |
|------|-------|
| 1994 | Shor publishes quantum factoring algorithm |
| 2016 | NIST launches Post-Quantum Cryptography standardization |
| Aug 2024 | FIPS 203 (ML-KEM), 204 (ML-DSA), 205 (SLH-DSA) finalized |
| Mar 2025 | HQC selected as 5th algorithm (will become FIPS 207) |
| 2025 | FIPS 206 (FN-DSA) expected |
| 2035 | **NIST deprecates all quantum-vulnerable algorithms** (NIST IR 8547) |

## What You Should Do Now

1. **Don't panic** — quantum computers capable of breaking crypto don't exist yet (estimated 2030s+)
2. **Do start planning** — migration takes years across large systems
3. **Prioritize key exchange** — this is where harvest-now-decrypt-later hits hardest
4. **Use hybrid mode** — run classical + PQC in parallel during transition (more on this in [Migration Guide]({% link 06-migration-guide.md %}))

**Next**: Learn about the [PQC algorithms]({% link 02-pqc-algorithms.md %}) that replace what's breaking.

---

**Sources**: [NIST PQC Project](https://csrc.nist.gov/projects/post-quantum-cryptography) | [NIST IR 8547 Transition Guidance](https://csrc.nist.gov/pubs/ir/8547/ipd)

*Last updated: 2026-02-13*
