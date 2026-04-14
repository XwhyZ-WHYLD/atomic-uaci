# UACI Framework — Architecture

## Overview

UACI operates as a five-module middleware stack positioned at the inference layer — between the AI model and output delivery. All five modules execute atomically in a single inference pass.

---

## Module 1 — UACI Protocol

The cryptographic identifier assigned to every piece of AI-generated content.

**Field Structure:**

| Field | Size | Description |
|-------|------|-------------|
| version | 1 byte | Protocol version — enables hash agility migration |
| timestamp_utc | 48 bits | UTC generation timestamp |
| model_fingerprint | BLAKE3-128 | Fingerprint of the generating model |
| content_hash | SHA256-128 | Hash of the raw generated content |
| CRC32 | 32 bits | Low-cost integrity check for non-crypto errors |

**Collision Resistance:** For n=10⁹ items, collision probability p < 10⁻¹⁸ under BLAKE3-128.

---

## Module 2 — StealthResist™ Watermarking

Invisible, robust watermark embedded across all content modalities.

| Modality | Domain | Target |
|----------|--------|--------|
| Image | DCT embedding | JPEG Q ≥ 30 resilience |
| Audio | STFT embedding | SNR drop ≤ 1 dB |
| Text | Token insertion | BLEU score ≤ 0.1 degradation |
| Video | Frame-level DCT | Roadmap item |

**Error Correction:** Three independent BCH(127, 64) codewords per asset — triple redundancy.

**Validated benchmark:** > 99.5% recovery after three Q=30 JPEG compressions on COCO 2025 dataset (25k images).

> ⚠️ Audio and video recovery benchmarks are roadmap items — target Q3 2026.

---

## Module 3 — PAIR Router

Inference-time middleware that orchestrates atomic binding of all three components.

**API surface:**
```
POST /inject
  Input:  raw_output (any modality)
  Output: { uaci_id, watermarked_content, p3_capsule }
```

**Integration hooks:**
- Hugging Face Transformers callback
- vLLM middleware

**Target latency:** ≤ 50ms median (CPU, Intel i7-1165G7)

---

## Module 4 — OpenVeri™ Toolkit

Open verification engine — any stakeholder can verify provenance without access to private keys.

**Verification workflow:**
```
extract watermark
  → validate UACI header
    → decrypt P3 vault (with authorised key)
      → verify HMAC
        → optionally export C2PA manifest
```

**REST endpoints:**
```
GET  /verify?uaci_id=&content=
POST /audit
```

**SDKs:** Python, JavaScript

---

## Module 5 — P3 Capsule

Privacy-preserving encrypted provenance vault. Contains metadata about content generation without exposing sensitive context.

**Schema:**
```json
{
  "uaci_id": "string",
  "model_name": "string",
  "timestamp_utc": "ISO-8601",
  "user_hash": "salted-SHA256",
  "intent_hash": "salted-SHA256",
  "hmac": "string"
}
```

**Encryption:** AES-GCM with 96-bit nonce per injection.

**Key management:** 90-day rotation via KMS. Failed rotations trigger audit alerts.

**GDPR compliance:**
- Erasure path: key destruction permanently prevents decryption
- Legal hold: key escrow for lawful subpoena compliance

---

## Data Flow

```
User Input
    │
    ▼
┌─────────────┐
│ PAIR Router │ ← Inference middleware
└──────┬──────┘
       │ atomic pass
       ├──────────────────────────────┐
       ▼                              ▼
┌─────────────┐              ┌───────────────┐
│   Model     │              │  UACI Protocol│
│  Inference  │              │  ID Generator │
└──────┬──────┘              └───────┬───────┘
       │                             │
       ▼                             ▼
┌─────────────┐              ┌───────────────┐
│StealthResist│              │  P3 Capsule   │
│  Watermark  │              │  Encryptor    │
└──────┬──────┘              └───────┬───────┘
       │                             │
       └──────────────┬──────────────┘
                      ▼
              Bound Output Asset
              { content + watermark + id + vault }
                      │
                      ▼
              OpenVeri™ (verification)
```
