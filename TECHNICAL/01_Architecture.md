# Technical Architecture — LLAMA_CPP

**Upstream:** [https://github.com/ggerganov/llama.cpp](https://github.com/ggerganov/llama.cpp)
**License:** MIT
**Category:** CHIP_QUANTIZATION
**Anticloud Integration:** PAX L5 Narrow L2 General 27B + AIOSS + Offline-First

## Upstream Description

CPU-optimized quantized LLM inference

## Anticloud Architectural Changes

1. PAX L5 Narrow L2 General 27B local accuracy recovery and calibration post-quantization
2. AIOSS provenance chain linking quantized model to original weights and calibration data
3. AES-256 encryption for proprietary model weights and calibration datasets
4. Single-binary quantization toolkit with no cloud API dependency
5. Zero-cloud: entire INT4/INT8/FP8 pipeline runs locally
6. GPU/CPU equalizer: quantization calibration on GPU, deployment testing on CPU
7. Open ONNX/GGUF export: no proprietary format lock-in
8. Automated accuracy benchmark: measures top-1 drop before deployment

## Integration Points

- **PAX Inference Socket:** Local HTTP endpoint at `127.0.0.1:11434/v1/chat` — same OpenAI-compatible API, zero cloud
- **AIOSS Hook:** Every write operation calls `aioss_append(event, payload)` before commit
- **Encryption Layer:** All file I/O routed through `anticloud_crypto.encrypt_at_rest()`
- **Single Binary Build:** `pyinstaller anticloud_llama_cpp.spec` or `go build -o llama_cpp`

## Deployment Modes

| Mode | Hardware | Notes |
| --- | --- | --- |
| Edge CPU | Raspberry Pi 4 / Intel NUC | Full feature set, PAX on CPU |
| Desktop GPU | RTX 3060 / A10 | PAX GPU inference, <1s latency |
| Server | 2× A100 | Full batch throughput |
| Air-gapped | Any x86/ARM | Zero network dependency |