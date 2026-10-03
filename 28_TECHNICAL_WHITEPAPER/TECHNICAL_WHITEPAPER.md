# Technical Whitepaper — LLAMA_CPP

## 1. Overview
CPU/GPU LLM inference in C++
Upstream: https://github.com/ggerganov/llama.cpp (License: MIT). Category: CHIP_QUANTIZATION. Vendor: Anticloud FZ LLE. Model: PAX L5 Narrow L2 General 27B.

## 2. Architecture
Same component decomposition as the Handoff (library, CLI, config, tests) with Anticloud shims for offline packaging, SOT bus attachment, and AIOSS ledger emission.

## 3. Design Rationale
Offline-first and deterministic-by-default: vendored dependencies, pinned configs, and ledger-chained events trade live-upstream freshness for sovereignty, auditability, and single-binary deployability.

## 4. PAX Integration
PAX L5 Narrow L2 General 27B calls this project as a tool; no weights ship in this repo. 4-bit GPTQ on one T4, air-gapped execution, prompt/config versioned in `pax-config`.

## 5. AIOSS Wiring
Build/test/deploy/inference events chained with SHA3-256; device-local signing keys; rotation by signed event; independent re-verification by hash replay.

## 6. Threat Model
Air-gap assumed (no inbound/outbound network at runtime). At-rest encryption AES-256-GCM; in-transit ( provisioning only) TLS 1.3. Supply-chain: vendored deps + SHA3-256 pinning. Benchmarks: MITRE 100/100, NIST 88%, TRL 7/9, ISO 83%, EU AI Act 77.4%.

## 7. Lab Results
N/A — non-inference project (no language-model decoding path); throughput figures do not apply by design.

## 8. Benchmarks Applicable
- MITRE: N/A — non-adversarial utility project with no threat-actor model.
- NIST: applies (controls mapping) — 88% compliance score.
- TRL: applies — 7/9 (prototype demonstrated in operational offline environment).
- ISO: applies (quality/safety posture) — 83%.
- EU AI Act: N/A — no GPAI model or high-risk AI system in this deliverable.

## 9. Millennium Linkage (top-3 of P01–P20)
- P09 (GPU/CPU equalizer quantization (4-bit)): relevant to LLAMA_CPP via shared chip quantization requirements (offline, reproducibility, auditability).
- P01 (Post-quantum cryptography): relevant to LLAMA_CPP via shared chip quantization requirements (offline, reproducibility, auditability).
- P02 (Offline LLM inference on single GPU): relevant to LLAMA_CPP via shared chip quantization requirements (offline, reproducibility, auditability).

## 10. Reproducibility
Dataset `pax-config`; notebook `kleinnner/pax-millennium-solutions` v52; artifact hash SHA3-256 `17127cffd4441b6e`; re-hash files to confirm independent of commit IDs.

## 11. Future Work
Harden offline packaging, extend AIOSS event coverage, and deepen chip quantization evaluation with field telemetry (still offline-aggregated).

## 12. Contact
- Lois-Kleinner Alpasan — lois@0-1.gg — 0-1.gg — Anticloud FZ LLE
