# TracePilot

**Evidence-driven AI incident investigation for Kubernetes and Java microservices.**

> **Status:** Public project proposal / MVP planning. The diagnostic engine is not implemented yet.

TracePilot aims to investigate production incidents by correlating **bounded, timestamped evidence**, generating verifiable root-cause hypotheses, and explicitly reporting uncertainty. The initial scope is Nginx Ingress HTTP 504 incidents affecting Kubernetes-hosted Java services.

## Why

A 504 reported at an Ingress may never reach the application. Diagnosing these failures means linking upstream timing, routing and endpoint changes, readiness transitions, and JVM signals. Simply pasting raw logs into an LLM is not reliable enough.

## v0.1 MVP

- **Offline-first:** upload sanitized evidence bundles; no live production credentials.
- **Deterministic parsing:** normalize Nginx Ingress timing and Kubernetes state evidence.
- **Timeline correlation:** align events and preserve source references.
- **Hypothesis-driven analysis:** rank possible causes only when supported by evidence.
- **Explicit uncertainty:** return `inconclusive` when evidence is stale or insufficient.
- **Replayable test cases:** synthetic incident bundles with known expected behavior.

### Out of scope for v0.1

- Live production cluster access or command execution
- Autonomous remediation
- Multitenant SaaS operations
- General-purpose monitoring dashboards

## Proposed stack

- **Core:** Python 3.12, Pydantic v2, pytest
- **API:** FastAPI
- **Frontend:** Next.js, TypeScript, Tailwind CSS
- **Local development:** Docker Compose

## Proposed repository layout

```text
apps/
  api/               # Incident API and investigation workflow (planned)
  web/               # Investigation workbench (planned)
packages/
  core/              # Deterministic parsers & correlation (planned)
fixtures/incidents/  # Reproducible incident evidence
scripts/             # Development and testing utilities (planned)
docs/
  architecture.md    # Architecture and safety boundaries
  roadmap.md         # Milestones and parallel Devin workstreams
```

## First demonstration goal

A developer uploads an incident bundle containing Nginx timing data and Kubernetes status evidence. TracePilot creates an evidence-linked timeline and returns either a justified hypothesis or an inconclusive result, without accessing any live cluster.

## Development approach

This project is planned for implementation with **Devin Bots** using contract-first development, separate workstream ownership, and automated acceptance tests. The documentation here is a plan, not a claim that a working system already exists.

See [architecture](docs/architecture.md) and [roadmap](docs/roadmap.md).

## Security principles

Logs and uploaded evidence are untrusted input. No production credentials, no arbitrary shell execution, and no automated repair. Any future live connector must be separately scoped to approved read operations.

## License

License to be selected before the first code release.
