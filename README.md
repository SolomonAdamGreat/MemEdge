# MemEdge

Lightweight conversational memory system optimized for **edge devices** (CPU-only).

## Current Results – Raspberry Pi 5 (CPU)

Evaluated on the full **LoCoMo** benchmark (1986 examples) using a **3B parameter model** running entirely on CPU:

| System                  | Accuracy | E2E Latency (avg) | Ingest (cache hit) | Wall Time    |
|-------------------------|----------|-------------------|--------------------|--------------|
| **MemEdge v4**          | **42.75%**   | **1.37s**             | **18 ms**              | **\~24.2 h**      |
| Simple RAG              | 42.0%    | 58.0s             | 13.0s              | \~32 h        |

- **100% conversation cache hit rate** across all 1986 examples
- **\~8 hours faster** total runtime than Simple RAG on the same hardware
- Significantly lower ingest latency thanks to conversation-level caching

→ Full detailed report (including validation set and previous Ollama results): [docs/locomo_benchmark.md](docs/locomo_benchmark.md)

## Key Strengths for Edge Use

- Designed for CPU / low-resource environments (robotics, industrial, defense)
- Very fast ingest on repeated conversations (critical on edge devices)
- Maintains competitive accuracy with much lower latency

## Reproduce (CPU)

```bash
python evaluate_locomo.py --mode memedge --split full \
  --llm-backend llama_cpp \
  --model-path /path/to/Qwen2.5-3B-Instruct-Q4_K_M.gguf
