# atomic-uaci

**Atomic-UACI: Inference-Time AI Content Provenance & Watermarking**

> ⚠️ **Status: Specification Stage (v2.1)** — This repository contains the formal technical specification and whitepaper for the UACI Framework. Implementation is in active development. Benchmarks reflect design targets validated at specification stage.

---

## What is UACI?

The UACI Framework™ (Universal AI Content Identification System) is the first framework to **atomically bind** a cryptographic identifier, invisible multimodal watermark, and encrypted provenance vault **during inference** — across text, image, audio, video, and code.

Most existing approaches handle provenance after generation — post-hoc manifests, separate watermark specs, or siloed per-media solutions. UACI addresses this at the inference layer, before output is released.

---

## The Problem

| Gap | Current State |
|-----|---------------|
| No standardised content identifiers | Each platform defines provenance differently |
| Watermarks stripped post-generation | No atomic binding at source |
| Opaque model pipelines | No inference-time auditability |
| Privacy exposure in provenance metadata | Plaintext manifests vulnerable |

---

## Five-Module Architecture

```
┌─────────────────────────────────────────────────┐
│                  UACI Framework                  │
├──────────┬──────────────┬────────────────────────┤
│  UACI    │ StealthResist│      PAIR Router        │
│ Protocol │  Watermark   │  (inference middleware) │
├──────────┴──────────────┴────────────────────────┤
│           OpenVeri Toolkit (verification)        │
├──────────────────────────────────────────────────┤
│              P3 Capsule (encrypted vault)        │
└──────────────────────────────────────────────────┘
```

| Module | Function |
|--------|----------|
| **UACI Protocol** | Cryptographic globally unique identifier — version, timestamp, model fingerprint, content hash, CRC32 |
| **StealthResist™** | Invisible multimodal watermark — DCT embedding for images, STFT for audio, token insertion for text |
| **PAIR Router** | Inference-time middleware — atomic injection of ID, watermark, and encrypted vault in a single pass |
| **OpenVeri™ Toolkit** | Open verification engine — extract watermark, validate header, decrypt vault, verify HMAC |
| **P3 Capsule** | Privacy-preserving encrypted container — AES-GCM provenance vault with GDPR erasure path |

---

## Key Differentiators

| Prior Art | Limitation | UACI Difference |
|-----------|-----------|-----------------|
| Google SynthID (US 11,706,256) | No encrypted vault; separate per-media specs | Unified atomic binding across all modalities |
| C2PA Spec v2.1 | Post-hoc cleartext manifests | Inference-time atomic binding |
| Intel GUID (US 11,514,365) | No watermark; post-generation only | Full cryptographic + watermark + vault pipeline |

---

## Performance Targets (Specification Stage)

| Operation | Median | 95th Percentile | Setup |
|-----------|--------|-----------------|-------|
| ID Generation | 3 ± 0.5 ms | 6 ± 1.2 ms | Intel i7-1165G7, 16GB RAM |
| Watermark Embed | 42 ± 5 ms | 68 ± 8 ms | CPU only, COCO 1080p |
| Verification | 55 ± 6 ms | 80 ± 10 ms | CPU/GPU NVIDIA T4 |
| Throughput (batch 4) | 250 req/s | — | 4-core VM |

> ⚠️ Image watermark benchmarks validated at specification stage. Audio and video recovery metrics are roadmap items — see [roadmap](docs/roadmap.md).

---

## Governance & Compliance Mapping

| Module | NIST SP 800-53 | ISO/IEC 42001 | EU AI Act / GDPR |
|--------|---------------|---------------|-----------------|
| UACI Protocol | SC-16, AU-10 | A.5, A.8 | Art 52 transparency |
| StealthResist | SC-7 | A.9 | — |
| P3 Capsule | MP-6, AC-17 | A.6 | GDPR Art 17 erasure |

---

## Repository Structure

```
atomic-uaci/
├── README.md
├── LICENSE
├── WHITEPAPER.md          — Full technical whitepaper v2.1
├── docs/
│   ├── architecture.md    — Five-module architecture detail
│   ├── security.md        — STRIDE threat model and mitigations
│   ├── compliance.md      — Governance and regulatory mapping
│   ├── benchmarks.md      — Performance specifications
│   └── roadmap.md         — Development roadmap
├── schemas/
│   └── p3-capsule.json    — P3 Capsule provenance schema
└── specs/
    └── uaci-protocol.md   — UACI Protocol field specification
```

---

## Roadmap

| Milestone | Target | Status |
|-----------|--------|--------|
| Specification v2.1 | Q1 2026 | ✅ Complete |
| POC Implementation | Q2 2026 | 🔄 In progress |
| Audio/Video Benchmarks | Q3 2026 | ⏳ Planned |
| Blockchain Registry Pilot | Q4 2026 | ⏳ Planned |
| ISO/IEC 42001 Engagement | Q1 2027 | ⏳ Planned |
| C2PA & W3C DID WG Contribution | Q1 2027 | ⏳ Planned |

---

## License

MIT License — see [LICENSE](LICENSE)

---

## Author

Roshan George Thomas
Independent AI Researcher — Bahrain
ORCID: [0009-0002-1175-7749](https://orcid.org/0009-0002-1175-7749)
Organisation: [XwhyZ-WHYLD](https://github.com/XwhyZ-WHYLD)
