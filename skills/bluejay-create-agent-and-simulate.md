---
name: Create an agent and run a simulation
description: >-
  Register a conversational AI agent in Bluejay, generate synthetic digital
  humans to test it, queue a simulation run, and read back the graded results.
api: openapi/bluejay-openapi-original.json
operations:
  - add_agent_v1_add_agent_post
  - generate_digital_humans_v1_generate_digital_humans_post
  - create_simulation_v1_create_simulation_post
  - queue_simulation_run_endpoint_v1_queue_simulation_run_post
  - get_simulation_runs_v1_get_simulation_runs__simulation_id__get
  - retrieve_simulation_results_v1_retrieve_simulation_results__simulation_run_id__get
---

# Create an agent and run a simulation

Base URL: `https://api.getbluejay.ai`. Authenticate every request with the
`X-API-Key` header (generate a key in Settings > API Keys). Errors come back as
HTTP 422 with a FastAPI `{ "detail": [...] }` body — inspect `detail[].loc` and
`detail[].msg` to fix a bad request. There is no idempotency key, so do not
blindly retry POSTs.

## Steps

1. **Register the agent** — `POST /v1/add-agent` (`add_agent_v1_add_agent_post`)
   with the agent's connection details. Keep the returned agent id.
2. **Generate digital humans** — `POST /v1/generate-digital-humans`
   (`generate_digital_humans_v1_generate_digital_humans_post`) for that agent id
   to synthesize realistic test callers, or create them explicitly.
3. **Create the simulation** — `POST /v1/create-simulation`
   (`create_simulation_v1_create_simulation_post`) grouping the digital humans
   into a test suite. Keep the returned simulation id.
4. **Queue a run** — `POST /v1/queue-simulation-run`
   (`queue_simulation_run_endpoint_v1_queue_simulation_run_post`). Keep the
   returned simulation_run_id.
5. **Poll runs** — `GET /v1/get-simulation-runs/{simulation_id}`
   (`get_simulation_runs_v1_get_simulation_runs__simulation_id__get`) until the
   run reaches a terminal status.
6. **Read results** — `GET /v1/retrieve-simulation-results/{simulation_run_id}`
   (`retrieve_simulation_results_v1_retrieve_simulation_results__simulation_run_id__get`)
   to get per-conversation grades, custom metrics, and evaluations. Prefer
   subscribing to the `simulation_result_evaluated` webhook instead of polling
   for scale (see asyncapi/bluejay-webhooks.yml).
