# Outward Claim Verification (posts, READMEs, launch content)

**Trigger:** "verify claims" — before anything outward-facing ships: LinkedIn post,
README, landing copy, release notes, conference slide
**Routing:** T2 extracts and checks; T1 only for judgment calls on softening;
publishing stays human — always.
**Why this template (from your own flow):** your marketing claims ground in canon
docs (control matrix, public-docs policy). This generalizes that rule: no outward
claim without a named grounding source — the discipline that prevents a public
walk-back.

## Prompt

```text
Verify the claims in this content before publication:
{{CONTENT}}
Grounding sources available: {{CANON — e.g. control matrix, policy docs,
benchmark results, the repo itself}}.

STAGE 1 — EXTRACT
Table every claim a skeptical reader could challenge: quantitative ("14
controls", "3x faster", "5 repos"), capability ("supports X", "works with Y"),
comparative ("more secure than"), and implied claims (a screenshot implies the
feature ships).

STAGE 2 — GROUND each claim:
- source: the specific doc/section, measurement, or repro command that backs it
- check: read or run it NOW — do not trust memory of what the source says
- status: VERIFIED (source in hand, current) · STALE (was true, world moved) ·
  UNGROUNDED (no source found) · OVERCLAIM (source supports a weaker statement)

STAGE 3 — RESOLVE
- VERIFIED → keep.
- STALE → update the number/statement to current, or cut it.
- OVERCLAIM → rewrite to exactly what the source supports. The weaker true
  claim is almost always still compelling.
- UNGROUNDED → cut, or do the work to create the grounding (run the benchmark,
  write the doc) BEFORE publishing. Never publish on "pretty sure".

STAGE 4 — OUTPUT
The claims table with statuses and sources · the rewritten content · a
diff-style list of what changed and why. Quantitative claims that survived get
their source noted alongside for next time.
```
