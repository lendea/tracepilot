# MVP Roadmap — Proposed Devin Bot Plan

## Day 1 — Lead Bot: Contracts and scaffolding

- Initialize monorepo for Python core, FastAPI and Next.js.
- Define typed `EvidenceBundle`, `EvidenceRecord`, `Incident`, `Hypothesis`, `EvidenceReference`, `InvestigationReport` models.
- Define versioned REST endpoints and validation rules.
- Add baseline CI and sample incident fixtures.
- Create three worker task briefs with nonoverlapping ownership.

## Parallel implementation

| Agent | Path ownership | Deliverable |
| --- | --- | --- |
| Lead (Bot 1) | `docs/`, `.github/`, integration changes | Contracts, CI, integration and reviews |
| Evidence worker (Bot 2) | `packages/core/` | Nginx parser, timeline correlation, coverage gaps |
| API worker (Bot 3) | `apps/api/` | Ingestion, typed workflow, evidence-cited hypotheses |
| Web worker (Bot 4) | `apps/web/` | Upload, timeline, hypothesis/coverage views |

Workers operate on separate branches and submit PRs. Shared contract changes require Lead review before merging.

## Demonstration acceptance criteria

1. Upload a synthetic 504 incident evidence bundle.
2. Display a reproducible timeline with source-linked observations.
3. Show ranked root-cause hypotheses only when supported by evidence.
4. For missing temporal evidence, return `inconclusive` instead of invented certainty.
5. Reject prompt injection embedded in log records and any execution requests.
6. Run unit, integration, and negative security tests locally.

## Current state

Planning artifacts only. These milestones are proposed and not yet implemented.
