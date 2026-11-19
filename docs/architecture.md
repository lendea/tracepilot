# TracePilot v0.1 — Architecture Proposal

## System objective

Investigate one incident class: Nginx Ingress HTTP 504 failures on Kubernetes-hosted Java microservices, using offline evidence bundles that can be replayed and evaluated.

## Component boundaries

1. **Evidence ingestion (`apps/api`)** — accepts uploaded bundles, validates file types/limits and redacts sensitive fields.
2. **Deterministic core (`packages/core`)** — parses Nginx access logs, normalizes timestamps, builds correlated observations, and tracks coverage gaps.
3. **Investigation workflow (`apps/api`)** — selects bounded evidence for an LLM, generates hypotheses, rejects uncited factual assertions, and returns `inconclusive` when needed.
4. **Workbench (`apps/web`)** — shows upload status, timeline, hypotheses, supporting records, and missing evidence.

## Example evidence contract (draft)

```json
{
  "schema_version": "0.1",
  "incident_id": "synthetic-ingress-504-001",
  "incident_window": {"start": "2026-11-19T14:32:00Z", "end": "2026-11-19T14:33:00Z"},
  "records": [
    {
      "id": "ingress-001",
      "source": "nginx-ingress-access-log",
      "observed_at": "2026-11-19T14:32:15Z",
      "collected_at": "2026-11-19T14:35:00Z",
      "data": {"status": 504, "upstream_addr": "10.42.0.15:8080", "request_time": 30.0}
    }
  ]
}
```

`observed_at` is the time associated with the event; `collected_at` is when the record was obtained. These must never be conflated.

## Key design tradeoff

The initial system uses post-incident, offline evidence instead of a streaming collector. This reduces the blast radius and makes tests reproducible, but it can miss transient readiness or endpoint changes. Healthy **post-incident** snapshots do not prove that a service was healthy **during** the incident.

Mitigations: separate event and collection timestamps; record coverage gaps; require explicit evidence references; return `inconclusive` when historical signals are missing.

## Safety model

- No Kubernetes credentials in the MVP.
- No arbitrary command execution or remediation tools.
- Validate and bound evidence size, file type, timestamp window, and parsing costs.
- Sanitize logs before model access; treat evidence as untrusted data, never instructions.
- Store evidence IDs for auditability; avoid placing raw secrets in prompt context.

## Later extensions (not part of v0.1)

Read-only, least-privilege connectors for Kubernetes API, Prometheus, and Loki; temporal collectors; tenant-level authorization; independent policy gateway. Live connectors must not change the deterministic core's evidence contract.
