# LoCoMo Benchmark Results (CPU-focused)

**Date:** 2026-07-03  
**Focus:** Edge / CPU performance with small models (\~3B parameters)  
**Model:** Qwen2.5-3B-Instruct (Q4_K_M GGUF via llama.cpp)

## Summary

MemEdge v4 was evaluated on the full LoCoMo benchmark (1986 examples) using a 3B parameter model on CPU only. The system achieved competitive accuracy while maintaining low latency and very efficient cache behavior.

## Full Set Results (1986 examples, CPU)

| System                  | Accuracy | Token F1 | E2E Latency (avg) | Ingest (cache hit) | LLM (avg) | Wall Time     |
|-------------------------|----------|----------|-------------------|--------------------|-----------|---------------|
| **MemEdge v4 (llama.cpp)** | **42.75%**   | 0.147    | **1.37s**             | **18 ms**              | 1.33s     | **45.4 min**      |

## Validation Set Results (25 examples, CPU)

| Backend              | Accuracy | Token F1 | E2E Latency (avg) | Ingest (cache hit) | LLM (avg) | Wall Time |
|----------------------|----------|----------|-------------------|--------------------|-----------|-----------|
| Ollama (CPU)         | \~40%     | \~0.10    | \~3.2s             | \~47 ms             | \~3.1s     | \~16 s     |
| **llama.cpp (CPU)**  | 36.0%    | 0.160    | **1.47s**             | 37 ms              | 1.41s     | 37 s      |

## Key Observations

- MemEdge v4 achieved **42.75% accuracy** on the full set with a 3B model running on CPU.
- Average end-to-end latency on the full set was **1.37 seconds**.
- Cache hit rate reached **100%** across all 1986 examples, with ingest time dropping to **18 ms** on cache hits.
- On the validation set, `llama.cpp` delivered significantly lower latency than Ollama while maintaining comparable accuracy.

## Reproduce (CPU)

```bash
# llama.cpp backend (recommended for edge)
python evaluate_locomo.py --mode memedge --split full \
  --llm-backend llama_cpp \
  --model-path /path/to/Qwen2.5-3B-Instruct-Q4_K_M.gguf
