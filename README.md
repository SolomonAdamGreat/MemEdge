# MemEdge

Lightweight, edge-optimized memory system for conversational agents.

## Current Benchmark Results

MemEdge was evaluated on the **LoCoMo** benchmark (full set, 1986 examples).

| System                  | Accuracy | E2E Latency (avg) | Notes                     |
|-------------------------|----------|-------------------|---------------------------|
| **MemEdge v4 (cache)**  | **42.5%**    | **2.78s**             | Best accuracy + speed     |
| Simple RAG              | 41.9%    | 4.44s             | -                         |
| Full Context            | 40.6%    | 3.22s             | Highest token usage       |

MemEdge achieved the highest accuracy while maintaining significantly lower average latency.

→ See the full detailed report: [docs/locomo_benchmark.md](docs/locomo_benchmark.md)

## Reproduce

```bash
python evaluate_locomo.py --mode all --split full
