---
name: Evaluate production calls with observability
description: >-
  Submit production voice/chat conversations to Bluejay for automated
  evaluation, define custom quality metrics, retrieve graded call logs, and set
  alerts on quality thresholds.
api: openapi/bluejay-openapi-original.json
operations:
  - evaluate_v1_evaluate_post
  - create_custom_metric_v1_create_custom_metric_post
  - retrieve_call_logs_v1_retrieve_call_logs__agent_id__get
  - retrieve_call_log_v1_retrieve_call_log__call_id__get
  - create_alert_v1_create_alert_post
---

# Evaluate production calls with observability

Base URL: `https://api.getbluejay.ai`. Authenticate with the `X-API-Key`
header. Validation failures return HTTP 422 with a `{ "detail": [...] }` body.

## Steps

1. **Define quality metrics** — `POST /v1/create-custom-metric`
   (`create_custom_metric_v1_create_custom_metric_post`) to declare the
   evaluation criteria (tone, accuracy, task completion) for an agent.
2. **Submit a call for evaluation** — `POST /v1/evaluate`
   (`evaluate_v1_evaluate_post`) with the production conversation (transcript /
   recording reference and metadata). Bluejay grades it against the agent's
   custom metrics.
3. **Retrieve graded call logs** — `GET /v1/retrieve-call-logs/{agent_id}`
   (`retrieve_call_logs_v1_retrieve_call_logs__agent_id__get`) for all calls, or
   `GET /v1/retrieve-call-log/{call_id}`
   (`retrieve_call_log_v1_retrieve_call_log__call_id__get`) for one; each call
   carries an `evaluations` array joined via `call_id`.
4. **Alert on regressions** — `POST /v1/create-alert`
   (`create_alert_v1_create_alert_post`) to fire when an observability metric
   crosses a threshold (routes to Slack/PagerDuty per your channel config).
5. **Automate ingestion** — subscribe to the `observability_call_evaluated`
   webhook (asyncapi/bluejay-webhooks.yml) to receive graded calls in real time
   instead of polling.
