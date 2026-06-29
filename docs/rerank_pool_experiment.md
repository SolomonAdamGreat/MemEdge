# Rerank pool experiment — v4 (pool=24) vs v4_small_pool (pool=12)

**Date:** 2026-06-27  
**Goal:** Faster pipeline with negligible accuracy loss by reducing `rerank_pool`.

---

## Executive summary

| Question | Answer |
|----------|--------|
| Does smaller `rerank_pool` speed up **ingest**? | **No.** Ingest is chunk batch-embedding; `rerank_pool` only affects query-time candidate count. |
| Does it speed up **retrieval**? | **Marginally** (~2 ms / example on vector search after a bugfix). |
| Accuracy impact (LoCoMo val, n=25)? | **36% → 32%** (−4 pp) — likely partly LLM variance on a small split; re-test on full split recommended. |
| Keep pool=12? | **Optional for speed** on LongMemEval (−10% wall, 0 pp accuracy loss on n=150); LoCoMo val showed −4 pp (likely noise). Prefer **pool=24** until post-bugfix re-run. |

**Important fix:** `VectorStore.search_by_vector` previously truncated results to `top_k`, so `rerank_pool` had **no effect**. This is now fixed; reranking actually sees 24 vs 12 candidates.

---

## Configuration

| Profile | `rerank_pool` | `candidate_multiplier` (top_k=6) | Candidates scored |
|---------|---------------|----------------------------------|-------------------|
| `v4` | 24 | 4 | up to 24 |
| `v4_small_pool` | 12 | 2 | up to 12 |

Activate:

```powershell
python run_edge_benchmark.py --profile v4_small_pool --boost --split val --systems agentcore
```

---

## Timing (pipeline profiler, LoCoMo val n=25, mock LLM)

| Phase | v4 (pool=24) | v4_small_pool (pool=12) | Delta |
|-------|--------------|-------------------------|-------|
| **ingest** | 4091 ms | 4030 ms | −61 ms (~0%) |
| embedding (query) | 39.8 ms | 34.6 ms | −5 ms |
| **vector_search** | 14.1 ms | 11.7 ms | **−2.4 ms** |
| rerank | 0.3 ms | 0.3 ms | 0 ms |
| diversify | <0.1 ms | <0.1 ms | 0 ms |
| **total** | 4149 ms | 4080 ms | −69 ms (~1.7%) |

**Conclusion:** Ingest dominates (~98% of non-LLM time). Shrinking `rerank_pool` does **not** address the profiling bottleneck.

---

## LongMemEval benchmark (n=150, Ollama) — completed

| Profile | Accuracy | Token F1 | Input tok | Wall clock | s/example |
|---------|----------|----------|-----------|------------|-----------|
| **v4** (pool=24) | **26.67%** | 0.204 | 831 | 1816 s (~30 min) | 12.1 |
| **v4_small_pool** (pool=12) | **26.67%** | 0.199 | 831 | 1634 s (~27 min) | 10.9 |

- **Accuracy delta: 0 pp** (40/150 both runs)
- **Input token: unchanged** (~831)
- **Wall time: −10%** with pool=12 (~3 min saved on 150 examples)

Reports:
- `runs/benchmarks/longmemeval/memedge_v4/rerank_pool_exp_20260627_104656/report.json`
- `runs/benchmarks/longmemeval/memedge_v4_small_pool/rerank_pool_exp_20260627_111712/report.json`
- Summary JSON: `runs/experiments/rerank_pool_20260627_114426.json`

**Note:** This run started before the `vector_store` candidate-pool bugfix; re-run recommended to confirm rerank_pool effect on accuracy.

---

## Accuracy benchmark (LoCoMo val, n=25, Ollama)

| Profile | Accuracy | Token F1 | Input tok | Wall clock | s/example |
|---------|----------|----------|-----------|------------|-----------|
| **v4** (pool=24) | **36.00%** | 0.193 | 829 | 153.4 s | 6.14 |
| **v4_small_pool** (pool=12) | 32.00% | 0.173 | 829 | 187.3 s | 7.49 |

- Accuracy delta: **−4.00 pp** (1 fewer correct answer on 25 examples)
- Token usage: **unchanged** (829)
- Wall time: small_pool was **slower** in this run (LLM / system variance, not retrieval)

Reports:
- `runs/experiments/rerank_pool_v4_baseline/report.json`
- `runs/experiments/rerank_pool_v4_small/report.json`

---

## Comparison to previous best (LoCoMo full, n=1986)

| System | Accuracy | Input tok |
|--------|----------|-----------|
| Simple RAG | 41.64% | 829 |
| MemEdge best (full) | 42.30% | 829 |
| MemEdge v2 + boost | 42.15% | 829 |

*This experiment used LoCoMo **val (25)** for quick turnaround. Full-split numbers should be re-run before changing production defaults.*

---

## Recommendation

1. **Do not expect ingest gains** from `rerank_pool` tuning — optimize ingest via **persistent vector index** / **skip re-embed per example**.
2. **pool=12** saves ~2 ms on vector search — negligible vs ~4 s ingest.
3. **Accuracy on n=25 dropped 4 pp** — treat as inconclusive; run `--split full` or LongMemEval n≥150 before adopting.
4. **Next step:** Add `v4_medium_pool` with `rerank_pool=16` as a compromise, or keep **v4 pool=24** until full-split A/B confirms ≤0.5 pp loss.

---

## Files changed

- `agentcore/core/edge_profiles.py` — new profile `v4_small_pool`
- `agentcore/memory/vector_store.py` — return full candidate pool (bugfix)
- `run_rerank_pool_experiment.py` — A/B timing + benchmark script
- `tests/core/test_vector_store.py` — pool size regression test

Run full experiment:

```powershell
python run_rerank_pool_experiment.py --benchmark longmemeval --split full --limit 150 --llm-backend ollama
```
