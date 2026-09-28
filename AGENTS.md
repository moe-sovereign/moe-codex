# Agent Instructions — MoE Codex

Always work toward the goal and the goal only.

## Engineering Rules

- Mind the coding rules, testing rules, and permanent memory before acting.
- No fallback path when the required path is unavailable.
- No translation layer unless explicitly requested.
- Keep it straight, simple, and excellent.
- If a seam fails the target design, rewrite it cleanly.
- No workarounds disguised as architecture.
- Test clean.
- Legacy code is deleted when replaced, or archived only when it must remain
  as explicit reference.
- Ask questions and challenge weak or underspecified input.
- Only start coding when open questions for the current seam are answered.

## Compliance Rules (Non-Negotiable)

- OPA policies in `policies/` are the access control source of truth.
  No authorization logic outside OPA policy files.
- BSI-Grundschutz and EU AI Act mapping docs in `docs/system/` must remain
  consistent with any feature that affects data handling, AI inference,
  or personal data processing — update them when relevant features change.
- GDPR Art. 32 + 35: any new data processing feature requires a DPIA assessment
  update in `docs/system/dsgvo_dpia_template.md`.
- Hosting: only EU-jurisdiction providers (Hetzner, IONOS, STACKIT, OVH, Scaleway).
  No US CLOUD-Act exposure. Air-gap deployment must remain possible.

## Dependency Rules

- MoE Codex is an optional add-on to MoE Sovereign. It communicates with
  `moe-sovereign` via HTTP/REST — no direct imports from moe-infra source.
- `SOVEREIGN_URL` configures the moe-sovereign connection (`services/sovereign_client.py`).
  Never hardcode the sovereign endpoint.
- All external service connections (Marquez, lakeFS, NiFi, Superset, OpenSearch,
  Falco, Trino, OPA, MLflow) use URL and credential env vars from `.env`.
  No hardcoded service addresses.

## Stack

- **API:** FastAPI (Python 3.11+), `main.py` + `routes/`
- **Services:** `services/` — one module per external system
- **Document intelligence:** `docling-service/` (separate container)
- **Database:** PostgreSQL via `services/` clients
- **Test command:** `pytest tests/ -v` (246 tests)
- **Rebuild:** `docker compose build <service> && docker compose up -d <service>`

## Complexity Decision Ladder

Before writing any new code, run through this sequence in order:

1. **Is this necessary?** YAGNI — does the system actually break without it?
2. **Does the stdlib provide it?** Python's stdlib, FastAPI, Pydantic first.
3. **Does an already-installed dependency cover it?** Check `requirements.txt`.
4. **Can it be one line?** If yes, write that line.
5. **Write the minimum that works.** No more.

**Never lazy about:**
- OPA policy completeness — authorization must be exhaustive (`policies/`)
- GDPR/BSI/EU AI Act compliance doc coverage — update when features change (`docs/system/`)
- License compliance — OSS-only stack, run `scripts/audit-licenses.sh` for new deps
- EU jurisdiction invariant — no data must leave EU-jurisdiction infrastructure
- Error handling against data loss or silent ETL failure
- Input validation at API boundaries

**`ponytail:` annotation:** When making an intentional simplification with a
known ceiling, mark it inline:
  `# ponytail: sync endpoint — upgrade to async when throughput > 100 req/s`

**Exempt from the ladder:** OPA policies (must be exhaustive), `docs/system/`
compliance mappings (must be complete), 246-test suite coverage (must stay
comprehensive).

## Testing Rules

- Prove backend/service behavior before GUI proof.
- Run `pytest tests/ -v` (246 tests) after any Python change.
- If adding a new route: add an integration test in `tests/`.
- `docling-service` changes require its own rebuild:
  `docker compose build docling-service && docker compose up -d docling-service`

## Backlog & Memory System

**Before starting any task:**

1. Load the AI Memory Pack: `docs/ai-memory/INDEX.md`
2. Read `docs/ai-memory/07-current-status-and-next-work.md` for current state.

**Note:** MoE Codex is feature-complete (D5 milestone, 2026-05-17).
New work should be decomposed as a new backlog initiative when scoped.
