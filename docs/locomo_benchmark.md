# LoCoMo Benchmark Results

**Date:** 2026-06-29  
**Benchmark:** LoCoMo  
**Model:** qwen2.5:3b (via Ollama)

## Overview

MemEdge v4 was evaluated on the LoCoMo benchmark against Simple RAG and Full Context baselines.

Results are presented for both the **full set** (1986 examples) and the **validation split** (25 examples).

## Full Set Results (1986 examples)

| System                  | Accuracy | Token F1 | E2E Latency (avg) | Ingest (avg) | LLM (avg) | Wall Time    | Cache Hit |
|-------------------------|----------|----------|-------------------|--------------|-----------|--------------|-----------|
| **MemEdge v4**          | **42.50%**   | 0.157    | **2.78s**             | **48 ms**        | 2.71s     | **1.53h**        | **100%**      |
| Simple RAG              | 41.89%   | 0.158    | 4.44s             | 1.70s        | 2.74s     | 2.45h        | —             |
| Full Context            | 40.58%   | 0.121    | 3.22s             | —            | 3.22s     | 1.77h        | —             |

## Validation Set Results (25 examples)

| System                  | Accuracy | Token F1 | E2E Latency (avg) | Wall Time |
|-------------------------|----------|----------|-------------------|-----------|
| **MemEdge v4**          | **40.0%**    | 0.182    | 3.07s             | 76.8s     |
| Simple RAG              | 36.0%    | 0.146    | 4.43s             | 110.8s    |
| Full Context            | 32.0%    | 0.184    | 17.64s            | 440.9s    |

## Observations

- MemEdge v4 recorded the highest accuracy on both the full set and validation set.
- MemEdge v4 showed the lowest average end-to-end latency on the full set.
- Full Context achieved the lowest Token F1 score on the full set.
- Simple RAG maintained competitive accuracy but had higher ingest latency compared to MemEdge v4.

## Detailed Reports

Full JSON and Markdown reports are available at:

- `runs/evaluation/locomo_full_with_cache/`
- `runs/evaluation/locomo_full_simple_rag/`
- `runs/evaluation/locomo_full_full_context/`

## Reproduce

```bash
python evaluate_locomo.py --mode all --split full
