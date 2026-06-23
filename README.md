# MemEdge Benchmark Report

**Lightweight Memory System for Edge AI**

This repository contains the first public benchmark results for **MemEdge**, a lightweight, edge-optimized memory system designed for resource-constrained environments (robotics, mobile, on-device agents).

## Summary

MemEdge was evaluated on two standard conversational memory benchmarks using a quantized 3B model (`qwen2.5:3b` Q4_K_M):

- **LoCoMo**: MemEdge achieved **44%** accuracy, significantly outperforming Simple RAG (24%) at the same token budget.
- **LongMemEval**: MemEdge reached **52%** accuracy while using **15% fewer tokens** than Simple RAG.

Full context approaches performed poorly on the 3B model, confirming that retrieval-based memory is essential at this scale.

## Full Report

For detailed results, tables, and analysis, see:

→ **[benchmark_report.md](benchmark_report_memedge.md)**

## Key Highlights

- Strong performance on mixed dialog QA (LoCoMo)
- Competitive results on ultra-long histories (LongMemEval)
- Excellent token efficiency (< 830 tokens vs 26k–127k full context)
- Designed for low-resource / edge deployment

## Reproduce

```bash
ollama pull qwen2.5:3b
python run_edge_benchmark.py --benchmark locomo --limit 25 --boost
python run_edge_benchmark.py --benchmark longmemeval --limit 50 --boost
