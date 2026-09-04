# Prompt Library

Copy-paste prompt templates that encode deep-reasoning discipline — the planning,
evidence, and verification steps that cheaper/faster models skip — so any model or
CLI (Claude, Codex, Gemini, ...) delivers close to top-tier quality at that tier's cost.

Two ideas run through every template:

1. **Plan expensive, execute cheap, verify at the tier of the risk.** Deep reasoning
   goes into plans, contracts, and adjudication. Cheap tiers do the labor inside
   those guardrails.
2. **No claim without evidence, no "done" without verification.** Every template
   forces pointers (file:line, run id, command output) and confidence labels, so
   weak-model guessing surfaces instead of hiding.

## Index — "when I say X, use Y"

| You say | Template | What it does |
|---|---|---|
| "fable plan" / "opus plan" | [01-model-routing](01-model-routing.md) + [02-plan-handoff](02-plan-handoff.md) | Top tier plans; cheap tier executes under contract |
| "sonnet build" / "haiku sweep" | [01-model-routing](01-model-routing.md) | Tiered execution bindings |
| "code review" | [10-code-review](10-code-review.md) | Evidence-first review, severity-gated verdict |
| "second cli review" / "use other cli for pr review" | [11-pr-review-second-cli](11-pr-review-second-cli.md) | Independent review in Codex/Gemini + reconciliation in Claude |
| "security review" | [12-security-review](12-security-review.md) | Threat-model-first, attack-path-required findings |
| "root cause ..." | [20-bug-root-cause](20-bug-root-cause.md) | Reproduce → falsify hypotheses → fix + regression test |
| "analyze ci" | [21-ci-pattern-analysis](21-ci-pattern-analysis.md) | Failure/flake/cost clustering with evidence thresholds |
| "extract friction" | [22-friction-extraction](22-friction-extraction.md) | Find patterns hurting progress, priced and prioritized |
| "design doc ..." | [30-design-doc](30-design-doc.md) | design.md with real options, non-goals, failure modes |
| "align docs" | [31-docs-alignment](31-docs-alignment.md) | Claims-table doc verification against source of truth |
| "capture learnings" | [40-learnings-capture](40-learnings-capture.md) | Post-run learnings that convert into repo canon |
| "verify claims" | [41-claim-verification](41-claim-verification.md) | Ground outward-facing claims before publishing |
| "decision session" / "close decisions" | [50-decision-partner](50-decision-partner.md) | Closes open product decisions on your own measurements, recorded with reopen bars |
| (preamble for any task) | [00-deep-validation-core](00-deep-validation-core.md) | The reasoning spine all templates inherit |

## Conventions

- **Placeholders**: `{{LIKE_THIS}}` — fill before pasting; `{{N=50}}` means
  default 50 unless you override.
- **Confidence labels** (shared vocabulary): `CONFIRMED` evidence in hand ·
  `PLAUSIBLE` mechanism identified, not reproduced · `SPECULATIVE` pattern-match
  only. Never present PLAUSIBLE as CONFIRMED.
- **Severity ladder**: `CRITICAL` block · `HIGH` should fix · `MEDIUM` consider ·
  `LOW` optional. (Matches the ECC code-review rules.)
- **Tiers**: `T1` = Fable/Opus (deep reasoning) · `T2` = Sonnet (workhorse) ·
  `T3` = Haiku (fast/cheap). Defined in [01-model-routing](01-model-routing.md).

## A full flow (example)

1. "fable plan" → T1 writes a plan per 02 (invariants, done-checks, stop conditions).
2. "sonnet build" → T2 executes the plan under the executor contract.
3. "code review" (T2) + "second cli review" (Codex) in parallel → two independent reviews.
4. Disagreements → T1 adjudicates with code evidence (Part B of template 11).
5. Ship → "capture learnings" (40); file canon updates in the repo, not in notes.

## Maintenance

Canon-first: this folder IS the canon for prompts. Improvements to a pattern get
edited here, not forked into parallel notes. Templates are standalone by design —
each embeds its own validation gates so it works pasted into any CLI without this
folder present. If a trigger phrase becomes habitual, it can graduate into a
Claude Code slash command that reads the template.
