# Current Status and Next Work — MoE Codex

Purpose: compact current-state file for context restore.

## Current Status (2026-06-17)

MoE Codex has reached the D5 milestone (feature-complete, 2026-05-17):

| Phase | Component | Status |
|---|---|---|
| 16 | OpenLineage / Marquez | ✅ |
| 17 | NiFi ETL submission | ✅ |
| 18 | lakeFS bundle versioning | ✅ |
| 19 | Enterprise-stack dashboard | ✅ |
| 20 | Data Catalog | ✅ |
| 21 | Branch-based Approval | ✅ |
| 22 | Read-only Cypher Explorer | ✅ |
| 23 | Data Health drift detection | ✅ |
| 24 | JupyterLite Notebook | ✅ |
| D1 | OPA, MLflow, NeMo Guardrails, Trino, DocLing | ✅ |
| D2 | Timeline, Link Analysis, Kestra, Forms, Search, Charts | ✅ |
| D3 | Superset, OpenSearch, Falco + OpenSCAP | ✅ |
| D4 | Budibase, TimescaleDB, HedgeDoc, PostGIS + KeplerGL | ✅ |
| D5 | Leaflet maps, Dossier/Case File, Doc corrections, pinned Docker | ✅ |

Coverage: 246 tests passing. 92% of Palantir modules covered.

## Next Work

No active sprint. The platform is feature-stable.

Potential follow-up areas (not yet scoped as backlog):

1. **D6 / New Module candidates** — based on operator feedback:
   - Temporal graph analysis (Neo4j time-slice queries)
   - Additional ETL connector types in NiFi
2. **Compliance updates** — if EU AI Act or BSI-Grundschutz release new versions.
3. **Performance / hardening** — when load testing reveals specific bottlenecks.

To start new work: create a new initiative under `docs/backlog/current/`
using the templates in `docs/backlog/templates/` (if backlog structure is
populated).

## Do Not Forget

- All external service connections via env vars — no hardcoded addresses.
- OPA policies in `policies/` are access control source of truth.
- License compliance: OSS-only stack (`scripts/audit-licenses.sh`).
- EU jurisdiction only — no US CLOUD-Act exposure.
- `pytest tests/ -v` (246 tests) must pass after any Python change.
- After any Python change: `docker compose build <service> && docker compose up -d <service>`
- `docling-service` has its own container and rebuild command.
- New compliance-relevant features require `docs/system/` mapping doc updates.
