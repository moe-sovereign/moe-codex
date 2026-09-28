# Project Compliance — MoE Codex

Purpose: compact standing contract for this project. This file wins over stale
historical notes.

## Authority Chain

Precedence (highest to lowest):

1. This file (`PROJECT_COMPLIANCE.md`)
2. `docs/ai-memory/` restore pack
3. `AGENTS.md` (engineering rules, compliance rules, stack)
4. `docs/system/` (BSI-Grundschutz, EU AI Act, GDPR, NIS2 mappings)
5. `docs/backlog/current/` (active backlog, when applicable)
6. Source code and code contracts
7. Archive / historical notes

## Restore Pack

After context loss, read in this order:

1. `docs/ai-memory/INDEX.md`
2. `docs/ai-memory/00-restore-protocol.md`
3. `docs/ai-memory/07-current-status-and-next-work.md`
4. task-specific source file or backlog sheet

## Project Execution Doctrine

- Work toward the goal and the goal only.
- No workaround architecture. If a seam fails the target design, rewrite it.
- No hardcoded external service addresses — all connections via env vars from `.env`.
- OPA policies in `policies/` are the access control source of truth.
- EU sovereignty is non-negotiable: all data processing must remain within
  EU-jurisdiction infrastructure. Air-gap deployment must remain possible.
- GDPR/BSI/EU AI Act compliance docs must be updated when relevant features change.
- Keep solutions straight, simple, and excellent.
- Delete replaced legacy code; archive only explicit reference material.

## Architecture Doctrine — Authority Model

| Layer | Authority | Notes |
|---|---|---|
| **Primary authority** | PostgreSQL (`terra_checkpoints`, data catalog entries) | Durable operational truth. |
| **Versioning truth** | lakeFS (bundle versions) | Immutable version history. |
| **Lineage truth** | Marquez (OpenLineage) | Data provenance records. |
| **Access control truth** | OPA policies (`policies/`) | Single source for authorization logic. |
| **Compliance truth** | `docs/system/` mapping docs | BSI-Grundschutz, EU AI Act, GDPR, NIS2. |
| **Derived / projection** | Superset, OpenSearch, Cytoscape, KeplerGL, Grafana | UI projections — not authority. |
| **Disposable** | Valkey (`terra_cache`), container filesystems | Rebuildfähig. |

**Key invariant:** MoE Codex is an optional add-on. It must never introduce
hard dependencies back into `moe-sovereign`'s source — HTTP/REST only.

## Testing Discipline

- `pytest tests/ -v` (246 tests) must pass after every Python change.
- Backend service behavior proven before browser/GUI proof.
- Compliance-relevant changes require manual verification of OPA policy
  evaluation and an update to the relevant `docs/system/` mapping doc.
- Rebuild: `docker compose build <service> && docker compose up -d <service>`

## Related Files

- `AGENTS.md` — engineering and compliance rules
- `docs/system/` — EU AI Act, BSI-Grundschutz, GDPR, NIS2, Palantir comparison
- `docs/ai-memory/` — context restore pack
- `README.md` — feature overview, quick start, architecture diagram
