Review the current diff (or the files I name) against the MoE Codex
Complexity Decision Ladder.

For each finding, output exactly:
- **File:line** — what was found
- **Ladder step violated** — which of the 5 steps was skipped
- **Suggested simplification** — concrete alternative (one line if possible)
- **Exempt?** — yes/no and reason if yes

Format: bullet list only. No preamble, no summary.
If nothing is overengineered, output exactly: "No findings — ladder satisfied."

**The ladder:**
1. Is this necessary? (YAGNI)
2. Does the stdlib/FastAPI/Pydantic provide it?
3. Does an already-installed dependency cover it?
4. Can it be one line?
5. Is this the minimum that works?

**Always exempt — do not flag:**
- OPA policies in `policies/` (must be exhaustive — authorization has no shortcuts)
- Compliance mapping docs in `docs/system/` (must be complete — GDPR/BSI/EU AI Act)
- License audit script `scripts/audit-licenses.sh` and its outputs
- EU jurisdiction enforcement logic (non-negotiable)
- Error handling against data loss or silent ETL failure
- Input validation at API trust boundaries
- Explicitly requested features
