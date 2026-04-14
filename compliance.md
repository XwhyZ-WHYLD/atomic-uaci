# UACI Framework — Governance & Compliance

## Regulatory Mapping

### NIST SP 800-53 Rev 5

| Module | Control | Description |
|--------|---------|-------------|
| UACI Protocol | SC-16 | Transmission of Security and Privacy Attributes |
| UACI Protocol | AU-10 | Non-Repudiation |
| StealthResist | SC-7 | Boundary Protection |
| P3 Capsule | MP-6 | Media Sanitisation (key destruction erasure path) |
| P3 Capsule | AC-17 | Remote Access (KMS-controlled vault access) |

### ISO/IEC 42001 (AI Management System)

| Module | Clause | Description |
|--------|--------|-------------|
| UACI Protocol | A.5 | AI system impact assessment |
| UACI Protocol | A.8 | AI system documentation |
| StealthResist | A.9 | AI system performance and monitoring |
| P3 Capsule | A.6 | AI system data governance |

### EU AI Act

| Module | Article | Requirement |
|--------|---------|-------------|
| UACI Protocol | Art 52 | Transparency obligations for AI-generated content — UACI ID provides machine-readable provenance disclosure |
| P3 Capsule | GDPR Art 17 | Right to erasure — key destruction path ensures compliance |

---

## C2PA Alignment

UACI is designed to be complementary to C2PA rather than competing with it.

**Key difference:** C2PA generates post-hoc signed manifests. UACI binds provenance atomically during inference.

**Interoperability:** OpenVeri™ verification workflow includes an optional C2PA manifest export step — enabling UACI-generated content to be verified within C2PA-compatible systems.

**Roadmap:** C2PA Working Group contribution planned Q1 2027.

---

## W3C DID Alignment

UACI Protocol identifiers are designed to be compatible with W3C Decentralised Identifier (DID) principles — verifiable, persistent, and resolvable without a central authority.

**Roadmap:** W3C DID Working Group contribution planned Q1 2027.

---

## Standards Engagement Roadmap

| Standard Body | Engagement | Target |
|---------------|------------|--------|
| ISO/IEC 42001 | Formal submission | Q1 2027 |
| C2PA Working Group | Technical contribution | Q1 2027 |
| W3C DID Working Group | Technical contribution | Q1 2027 |
| NIST AI RMF | Crosswalk via UPIF | ✅ Submitted April 2026 |
