---
layout: default
title: Home
nav_order: 1
description: "Post-Quantum Cryptography for Engineers — understand and use PQC with confidence"
permalink: /
---

# PQC Engineering Guide

Quantum computers will break the cryptography protecting your applications. This guide helps you understand what's changing, choose the right replacement algorithms, and plan your migration — all without a PhD in mathematics.

## Who This Guide Is For

You're a **backend or infrastructure engineer** who understands TLS, SSH, and key management. You don't need to know lattice theory or formal security proofs. You need to know:

- What's breaking and when
- What to replace it with
- How to do it in your stack

## Guide Contents

| # | Section | What You'll Learn |
|---|---------|-------------------|
| 1 | [Why PQC Matters]({% link 01-why-pqc.md %}) | The quantum threat, what breaks, and the 2035 deadline |
| 2 | [PQC Algorithms]({% link 02-pqc-algorithms.md %}) | The 5 NIST-standardized algorithms and when to use each |
| 3 | [Algorithm Comparison]({% link 03-algorithm-comparison.md %}) | Key sizes, speeds, and memory — engineering data tables |
| 4 | [Integration Patterns]({% link 04-integration-patterns.md %}) | How PQC fits into TLS, SSH, and VPN protocols |
| 5 | [Libraries & Tools]({% link 05-libraries-and-tools.md %}) | PQC libraries for Rust, Go, Python, Java, C/C++, and JS |
| 6 | [Migration Guide]({% link 06-migration-guide.md %}) | Four-phase roadmap from classical crypto to PQC |
| 7 | [Glossary]({% link 07-glossary.md %}) | PQC terms explained in plain engineering language |

## Quick Decision Tree

```mermaid
flowchart TD
    A[What do you need?] --> B{Key exchange<br/>or signing?}
    B -->|Key Exchange| C[ML-KEM<br/>FIPS 203]
    B -->|Digital Signatures| D{Signature size<br/>priority?}
    D -->|Smallest signatures| E[ML-DSA<br/>FIPS 204]
    D -->|Smallest keys| F[SLH-DSA<br/>FIPS 205]
    D -->|Balanced| G[FN-DSA<br/>FIPS 206]
    A --> H{Backup KEM<br/>different family?}
    H -->|Yes| I[HQC<br/>FIPS 207]

    classDef dark fill:#6b8e6b,stroke:#4a6a4a,color:#2c3e50
    classDef accent fill:#5a8eaa,stroke:#3a6e8a,color:#2c3e50
    classDef highlight fill:#8e6b8e,stroke:#6e4a6e,color:#2c3e50
    classDef warm fill:#8e7060,stroke:#6e5040,color:#2c3e50

    class A dark
    class B accent
    class C,I highlight
    class D accent
    class E,F,G warm
    class H accent
```

## How to Read This Guide

- **In a hurry?** Read [Why PQC Matters]({% link 01-why-pqc.md %}) (10 min) and the [Algorithm Comparison]({% link 03-algorithm-comparison.md %}) tables.
- **Choosing a library?** Jump to [Libraries & Tools]({% link 05-libraries-and-tools.md %}).
- **Planning a migration?** Start with the [Migration Guide]({% link 06-migration-guide.md %}).
- **Reading end-to-end?** Follow sections 1 through 7 in order.

---

*Last updated: 2026-02-13*
