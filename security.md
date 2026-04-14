# UACI Framework — Security & Threat Analysis

## Threat Model

UACI assumes an adversary with the following capabilities:

- Access to the generated output (but not the private key vault)
- Ability to apply standard transformations — compression, re-encoding, paraphrasing, format conversion
- Ability to attempt replay attacks using previously captured UACI IDs

UACI does not assume:

- Protection against an adversary with physical access to KMS private keys
- Resilience against adversaries who can modify the PAIR Router itself

---

## STRIDE Analysis

| Threat | Vector | Mitigation |
|--------|--------|------------|
| **Spoofing** | ID-watermark replay attack using captured UACI ID | Unique nonce per injection; server-side duplication checks reject replayed IDs |
| **Tampering** | Header stripping or watermark removal | Atomic binding — ID, watermark, and vault are cryptographically linked; ECC triple redundancy in watermark |
| **Repudiation** | Denial of content generation | UACI ID + P3 Capsule HMAC provides non-repudiable generation record |
| **Information Disclosure** | Vault plaintext leak | AES-GCM encryption; KMS audit logs; salted hashing of user_hash and intent_hash |
| **Denial of Service** | KMS overload during key rotation | Async key cache; failed rotations trigger audit alerts before service impact |
| **Elevation of Privilege** | Unauthorised vault decryption | AES-GCM with 96-bit nonce; key rotation every 90 days; legal hold via key escrow |

---

## Attack Vectors and Mitigations

### Compression Attacks
**Vector:** JPEG recompression at low quality settings to destroy watermark.
**Mitigation:** BCH(127, 64) triple redundancy — validated > 99.5% recovery at Q=30 triple compression.

### Re-encoding Attacks
**Vector:** Format conversion (JPEG → PNG → JPEG) to degrade watermark signal.
**Mitigation:** Spread-spectrum DCT embedding distributes signal across frequency components — resilient to format-preserving re-encoding.

### Paraphrasing Attacks (Text)
**Vector:** AI-assisted paraphrasing to remove token-level watermark.
**Mitigation:** Token insertion targets semantic anchors rather than surface tokens — partial resilience. Full robustness against advanced paraphrase attacks is a roadmap item.

### Key Compromise
**Vector:** Adversary obtains AES-GCM encryption key.
**Mitigation:** 90-day key rotation; KMS audit logging; compromised key window limited to rotation period.

### Nonce Reuse
**Vector:** Nonce collision enabling ciphertext comparison attacks.
**Mitigation:** 96-bit random nonce per injection — collision probability negligible at operational scale.

---

## GDPR Compliance Paths

### Right to Erasure (Art 17)
Key destruction permanently prevents decryption of associated P3 Capsule. Content watermark remains intact — only the private provenance metadata becomes inaccessible.

### Legal Hold
Key escrow enables lawful decryption under subpoena without exposing the general key management system.

### Data Minimisation
user_hash and intent_hash are salted SHA-256 digests — original values are never stored in the vault. Salt rotated every 30 days.

---

## Pending Security Work

| Item | Status |
|------|--------|
| Full STRIDE table (all 12 subcategories) | Roadmap — Q2 2026 |
| Full LINDDUN privacy threat table | Roadmap — Q2 2026 |
| Formal DPO erasure SOP documentation | Roadmap — Q2 2026 |
| Audio/video transformation attack testing | Roadmap — Q3 2026 |
| Diffusion model resilience testing | Roadmap — Q3 2026 |
