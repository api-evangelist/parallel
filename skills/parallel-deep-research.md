---
name: Run a deep-research Task and fetch the result
description: Create a Parallel Task run for a research objective, poll its status, and retrieve the completed markdown result.
api: openapi/parallel-openapi-original.json
operations:
  - tasks_runs_post_v1_tasks_runs_post
  - tasks_runs_get_v1_tasks_runs__run_id__get
  - tasks_runs_result_get_v1_tasks_runs__run_id__result_get
---

# Run a deep-research Task and fetch the result

Authenticate with the `x-api-key` header. Base URL `https://api.parallel.ai`.

## Steps

1. **Create the run** — `POST /v1/tasks/runs` (`tasks_runs_post_v1_tasks_runs_post`).
   Provide the task spec (input, output schema) and a `processor` tier
   (`lite`, `base`, `core`, `pro`, `ultra`) sized to the complexity. You receive a
   `run_id`.
2. **Track progress** — either poll `GET /v1/tasks/runs/{run_id}`
   (`tasks_runs_get_v1_tasks_runs__run_id__get`) or subscribe to the SSE event
   stream at `GET /v1/tasks/runs/{run_id}/events`.
3. **Get the result** — `GET /v1/tasks/runs/{run_id}/result`
   (`tasks_runs_result_get_v1_tasks_runs__run_id__result_get`). This blocks until
   the run completes and returns the structured result plus research basis
   (citations, reasoning, confidence).

## Rules

- Task creates are limited to 2,000/minute; GET status/result calls are not counted.
- You are only billed for successfully completed runs; failed runs are not billed.
- A `408` on the blocking result means the wait timed out but the run is still
  active — resume polling or use SSE. See `errors/parallel-problem-types.yml`.
- For batches, use Task Groups (`tasks_taskgroups_post_v1_tasks_groups_post`).
