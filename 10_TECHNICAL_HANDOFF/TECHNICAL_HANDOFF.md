# Technical Handoff — LLAMA_CPP

## 1. Overview
CPU/GPU LLM inference in C++
Upstream: https://github.com/ggerganov/llama.cpp (License: MIT). This project is integrated into Anticloud FZ LLE as an offline-first, single-binary deliverable under `E:/fenta/Downloads/The Anticloud`.

## 2. Architecture
Upstream components: core library, CLI entry point, configuration layer, test suite. Anticloud integration points: `anticloud_core` runtime shim, `INSTALLER` single-binary packager, `19_SYSTEM_OF_THINGS_SOT` device bus, `27_DEPENDENCIES` vendored offline mirror. All paths stay under `E:/fenta/Downloads/The Anticloud`.

## 3. PAX Integration
Inference path: no model decoding path; PAX L5 Narrow L2 General 27B may invoke this project as a deterministic tool. Model: PAX L5 Narrow L2 General 27B, 4-bit GPTQ quantization, single NVIDIA T4, fully offline (no network egress).

## 4. AIOSS Wiring
Lifecycle events (build, test, deploy, inference-chunk) are chained into AIOSS ledgers. Each event record carries prev-hash + SHA3-256 digest. Key policy: device-local Ed25519 signing keys, public keys pinned in-repo; rotation via signed rotation event. Verification: replay chain from genesis, check `2828cffabd1d063a`-style head pointer.

## 5. Lab Results
N/A — non-inference project (no language-model decoding path); throughput figures do not apply by design.

## 6. Reproducibility
Dataset: `pax-config` (pinned config snapshot). Notebook: `kleinnner/pax-millennium-solutions` v52. Commit-agnostic file hashes: SHA3-256 `671f9b6d421076ec` recorded per artifact; re-hash to verify regardless of VCS commit.

## 7. Integration Test Plan
- Offline import/smoke test: load the vendored package with no network (assert no socket egress).
- AIOSS event test: emit a build event and verify chain continuity (prev-hash links, SHA3-256 digest matches recomputation).
- Determinism test: run the project's self-test suite twice and diff outputs (must be byte-identical excluding timestamps).
- Single-binary test: package via `INSTALLER` and execute on a clean offline VM (Ubuntu 22.04, no internet).

## 8. Operational Notes
Logging is structured JSON (event, level, correlation id). Retries are bounded with jitter. Caches are content-addressed by SHA3-256. Upgrades are atomic file swaps with rollback to the previous pinned snapshot.

## 9. Contact
- Maintainer: Lois-Kleinner Alpasan — lois@0-1.gg
- Site: 0-1.gg — Anticloud FZ LLE
