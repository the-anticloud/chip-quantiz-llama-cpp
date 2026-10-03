# Build and Test

**Project:** `LLAMA_CPP`
**Upstream:** https://github.com/ggerganov/llama.cpp
**License:** MIT

## Quick Start

```bash
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp
pip install -r requirements-anticloud.txt
python anticloud_main.py --offline --pax-local
```

## Anticloud Improvements Applied

1. PAX L5 Narrow L2 General 27B local accuracy recovery and calibration post-quantization
2. AIOSS provenance chain linking quantized model to original weights and calibration data
3. AES-256 encryption for proprietary model weights and calibration datasets
4. Single-binary quantization toolkit with no cloud API dependency
5. Zero-cloud: entire INT4/INT8/FP8 pipeline runs locally
6. GPU/CPU equalizer: quantization calibration on GPU, deployment testing on CPU
7. Open ONNX/GGUF export: no proprietary format lock-in
8. Automated accuracy benchmark: measures top-1 drop before deployment

## Benchmark Targets

| Metric | Target |
| --- | --- |
| Latency | Primary inference task: <5s on CPU, <1s on GPU |
| Throughput | Batch processing: >100 items/hour on single CPU server |
| Memory | <8GB RAM for standard deployment |
| Accuracy | Task-specific accuracy within 5% of cloud-API baseline |

## Build Status

Not yet measured. Run verified build and record actual figures above.
