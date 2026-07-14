## Full Set Results (1986 examples, CPU)

| System                        | Accuracy | E2E Latency (avg) | Ingest (cache hit) | LLM (avg) | Wall Time    |
|-------------------------------|----------|-------------------|--------------------|-----------|--------------|
| **MemEdge v4 (llama.cpp)**    | **42.75%**   | **1.37s**             | **18 ms**              | 1.33s     | **45.4 min**     |
| Simple RAG (previous)         | 41.89%   | \~4.4s             | \~1.7s              | \~2.7s     | \~2.45h       |
| Full Context (previous)       | 40.58%   | \~3.2s             | —                  | \~3.2s     | \~1.8h        |

## Validation Set Results (25 examples, CPU)

| System                        | Accuracy | E2E Latency (avg) | Ingest (cache hit) | LLM (avg) |
|-------------------------------|----------|-------------------|--------------------|-----------|
| **MemEdge v4 (llama.cpp)**    | 36.0%    | **1.47s**             | 37 ms              | 1.41s     |
| MemEdge v4 (Ollama, previous) | \~40%     | \~3.2s             | \~47 ms             | \~3.1s     |
| Simple RAG (previous)         | 36.0%    | \~4.4s             | —                  | —         |
| Full Context (previous)       | 32.0%    | \~17.6s            | —                  | —         |
