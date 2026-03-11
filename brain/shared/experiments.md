# Experiment Log

Structured log of everything tried: what worked, what didn't, and why.

**Every experiment MUST be logged here**, even failures. This is how we avoid repeating mistakes.

---

## Log

| # | Date | Topic | What was tried | Result | Status | Sources |
|---|------|-------|----------------|--------|--------|---------|
| _(example)_ | 2026-03-11 | [01_api-design](../topics/01_api-design.md) | Tried REST vs GraphQL for user endpoint | REST 3x faster for our use case | keep | [bench](../workspace/02_api-benchmark.md) |
| _(example)_ | 2026-03-11 | [01_api-design](../topics/01_api-design.md) | Tried gRPC for internal services | Added complexity, no speed gain | discard | [analysis](../workspace/03_grpc-test.md) |

### Status values:
- **keep** — worked, result is being used
- **discard** — didn't work or not worth the tradeoff
- **partial** — partially useful, needs more investigation
- **crash** — failed to complete (error, broken, etc.)

---

## Insights

Patterns noticed across experiments:

_(Fill as patterns emerge — e.g., "approaches X tend to fail because Y")_
