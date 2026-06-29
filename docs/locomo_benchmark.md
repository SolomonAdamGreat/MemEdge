# LoCoMo Benchmark Results

**Date:** 2026-06-29  
**Benchmark:** LoCoMo (full set + validation split)  
**Model:** qwen2.5:3b (via Ollama)

## Summary

MemEdge v4 was evaluated against Simple RAG and Full Context baselines on the LoCoMo benchmark.

On the **full set** (1986 examples), MemEdge v4 achieved the highest accuracy while maintaining the lowest average end-to-end latency.

## Full Set Results (1986 examples)

| System                  | Accuracy | Token F1 | E2E Avg | Ingest   | LLM     | Wall Time     | Cache Hit |
|-------------------------|----------|----------|---------|----------|---------|---------------|-----------|
| **MemEdge v4 (cache)**  | **42.50%**   | 0.157    | **2.78s**   | **48 ms**    | 2.71s   | **1.53h**         | **100%**      |
| Simple RAG              | 41.89%   | 0.158    | 4.44s   | 1.70s    | 2.74s   | 2.45h         | n/a           |
| Full Context            | 40.58%   | 0.121    | 3.22s   | 0 ms     | 3.22s   | 1.77h         | n/a           |

## Validation Set Results (25 examples)

| System                  | Accuracy | Token F1 | E2E Avg | Wall Time |
|-------------------------|----------|----------|---------|-----------|
| **MemEdge v4 (cache)**  | **40.0%**    | 0.182    | 3.07s   | 76.8s     |
| Simple RAG              | 36.0%    | 0.146    | 4.43s   | 110.8s    |
| Full Context            | 32.0%    | 0.184    | 17.64s  | 440.9s    |

## Key Metrics

- **Highest accuracy**: MemEdge v4 (both full set and validation set)
- **Lowest average latency**: MemEdge v4 (full set)
- **Full Context** showed the lowest Token F1 on the full set (0.121)
- Simple RAG maintains competitive accuracy but has significantly higher ingest time

## Reports

Detailed JSON and Markdown reports are available in:

- `runs/evaluation/locomo_full_with_cache/`
- `runs/evaluation/locomo_full_simple_rag/`
- `runs/evaluation/locomo_full_full_context/`

## Reproduce

```bash
python evaluate_locomo.py --mode all --split full
