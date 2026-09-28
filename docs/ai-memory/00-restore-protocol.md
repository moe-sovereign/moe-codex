# Restore Protocol — MoE Codex

## Immediate Behavior

When context is lost, reload memory in this order:

1. `INDEX.md`
2. this file
3. `07-current-status-and-next-work.md`
4. task-specific source or backlog files

Do not restart from scratch if restored memory already anchors the current
direction. Continue from the current goal and verify against active rules.

## Working Contract

- Work toward the goal and the goal only.
- No workaround architecture. Rewrite cleanly when a seam fights the target.
- No hardcoded external service addresses — all connections via env vars.
- OPA policies in `policies/` are the access control source of truth.
- EU sovereignty is non-negotiable. All data processing stays within EU
  infrastructure. Air-gap deployment must remain possible after any change.
- Update `docs/system/` compliance mapping docs when relevant features change.
- Delete replaced legacy code. Archive only explicit reference material.
- Test clean: `pytest tests/ -v` (246 tests) must pass after any Python change.
- Ask questions and challenge weak or underspecified input.

## Compliance Invariants (Always Active)

- **GDPR Art. 32 + 35**: new personal data processing features require a DPIA
  assessment update in `docs/system/dsgvo_dpia_template.md`.
- **EU AI Act** risk-class mapping (`docs/system/eu_ai_act_mapping.md`) must
  reflect any change to AI inference or automated decision-making components.
- **BSI-Grundschutz** baustein mapping (`docs/system/bsi_grundschutz_mapping.md`)
  must reflect any change to security-relevant components.
- **License compliance**: new dependencies must use OSI-approved open-source
  licenses. Run `scripts/audit-licenses.sh` and update
  `docs/system/license_compliance.md` when adding dependencies.
  Use OSS alternatives: OpenSearch (not ES), Valkey (not Redis ≥7.4),
  OpenTofu (not Terraform).

## Deploy Rule

After any Python change to a service:
```
docker compose build <service> && docker compose up -d <service>
```

`docling-service` has its own container:
```
docker compose build docling-service && docker compose up -d docling-service
```

## Current Status Reference

See `07-current-status-and-next-work.md` for the current milestone and
open follow-up items.
