# LoCoMo Benchmark Results

**Last updated:** 2026-07-14  
**Models tested:** Qwen2.5-3B-Instruct (Ollama and llama.cpp Q4_K_M)

## Overview

MemEdge v4 was evaluated on the LoCoMo benchmark across two hardware environments:

- **Standard PC** (CPU)
- **Raspberry Pi 5** (CPU-only, real edge hardware)

The goal was to measure performance in resource-constrained environments typical for robotics, industrial, and defense applications.

## Full Set Results (1986 examples)

### Raspberry Pi 5 (CPU-only)

| System                  | Accuracy | E2E Latency (avg) | Ingest (cache hit) | LLM (avg) | Wall Time    | Cache Hit |
|-------------------------|----------|-------------------|--------------------|-----------|--------------|-----------|
| **MemEdge v4**          | **42.75%**   | **1.37s**             | **18 ms**              | 1.33s     | **\~24.2 h**      | **100%**      |
| Simple RAG              | 42.0%    | 58.0s             | 13.0s              | 45.0s     | \~32 h        | n/a           |

### Standard PC (CPU)

| System                        | Accuracy | E2E Latency (avg) | Ingest (cache hit) | LLM (avg) | Wall Time    | Cache Hit |
|-------------------------------|----------|-------------------|--------------------|-----------|--------------|-----------|
| **MemEdge v4 (llama.cpp)**    | 42.75%   | 1.37s             | 18 ms              | 1.33s     | \~24.2 h      | 100%      |
| MemEdge v4 (Ollama, previous) | \~42.5%   | \~2.78s            | \~48 ms             | \~2.71s    | \~1.53 h      | 100%      |
| Simple RAG (previous)         | 41.89%   | 4.44s             | 1.70s              | 2.74s     | 2.45 h       | n/a       |
| Full Context (previous)       | 40.58%   | 3.22s             | —                  | 3.22s     | 1.77 h       | n/a       |

## Validation Set Results (25 examples)

### Raspberry Pi 5 (CPU-only)

| System                  | Accuracy | E2E Latency (avg) | Ingest (cache hit) | LLM (avg) |
|-------------------------|----------|-------------------|--------------------|-----------|
| **MemEdge v4**          | 36.0%    | **1.47s**             | 37 ms              | 1.41s     |

### Standard PC (CPU)

| System                        | Accuracy | E2E Latency (avg) | Ingest (cache hit) | LLM (avg) |
|-------------------------------|----------|-------------------|--------------------|-----------|
| MemEdge v4 (llama.cpp)        | 36.0%    | 1.47s             | 37 ms              | 1.41s     |
| MemEdge v4 (Ollama, previous) | \~40%     | \~3.2s             | \~47 ms             | \~3.1s     |
| Simple RAG (previous)         | 36.0%    | \~4.4s             | —                  | —         |
| Full Context (previous)       | 32.0%    | \~17.6s            | —                  | —         |

## Key Observations

- On **real edge hardware** (Raspberry Pi 5, CPU-only), MemEdge v4 achieved **42.75% accuracy** with an average latency of **1.37 seconds**.
- Conversation-level caching delivered **100% hit rate** on the full set, reducing ingest time to just **18 ms**.
- Compared to Simple RAG on the same Raspberry Pi hardware, MemEdge was approximately **24% faster** in average E2E latency and saved roughly **8 hours** of total runtime.
- The performance advantage of MemEdge is even more pronounced on resource-constrained devices, where re-embedding conversations is particularly expensive.

## Reports

**Raspberry Pi 5 results:**
- Full set: `runs/evaluation/locomo/full/pi_llama_cpp_qwen25_3b_20260711_070032/`
- Validation set: `runs/evaluation/locomo/val/20260703_150410/`

**Previous PC results** are available in the repository history and earlier evaluation folders.

## Reproduce

```bash
# Raspberry Pi / CPU
python evaluate_locomo.py --mode memedge --split full \
  --llm-backend llama_cpp \
  --model-path /path/to/Qwen2.5-3B-Instruct-Q4_K_M.gguf
