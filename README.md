# MemEdge

Lightweight, edge-optimized memory system for conversational agents.

## Current Benchmark Results

MemEdge was evaluated on the **LoCoMo** benchmark (full validation set, 1986 examples).

- **MemEdge v4 (with cache)**: **42.5%** accuracy
- Simple RAG: 41.9% accuracy
- Full Context: 40.6% accuracy

MemEdge achieved the best accuracy while being significantly faster thanks to conversation-level caching.

→ See the full report: [docs/locomo_benchmark.md](docs/locomo_benchmark.md)

## Reproduce

```bash
python evaluate_locomo.py --mode all --split full
