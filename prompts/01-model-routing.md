# Model Routing Policy

**Trigger phrases:** "fable plan" / "opus plan" · "sonnet build" · "haiku sweep" · "adjudicate"
**Goal:** best achievable quality per dollar — deep reasoning where it compounds
(plans, contracts, adjudication), cheap tiers where guardrails carry the quality.

## Tiers

| Tier | Models | Use for | Never for |
|---|---|---|---|
| T1 | Fable / Opus | Planning, architecture, ambiguous requirements, root cause on gnarly bugs, security adjudication, final review of high-risk diffs, resolving reviewer disagreement | Running commands, mechanical edits, first-pass summarization |
| T2 | Sonnet | Implementation from a T1 plan, standard code review, test writing, refactors inside one module, docs drafting, CI triage | Architecture decisions while requirements are still ambiguous |
| T3 | Haiku | Renames, formatting, boilerplate, running commands + reporting output, log/first-pass triage, bulk summarization | Severity adjudication, irreversible actions, anything without a stated done-check |

## Routing rules

1. **Plan expensive, execute cheap.** T1 output is a plan per
   [02-plan-handoff](02-plan-handoff.md); T2/T3 execute under its contract.
   Reasoning bought once at plan time is reused for free at every step.
2. **Verification tier = risk tier, not effort tier.** A trivial-looking diff to
   auth, payments, migrations, or a public API gets T1 review even if T3 wrote it.
3. **Escalate one tier when:** two failed attempts at the same error · plan-reality
   mismatch discovered mid-execution · the diff grows past the plan's stated scope ·
   two reviewers disagree on severity. Security or data-loss implications skip
   the ladder — straight to T1.
4. **De-escalate when:** a plan is approved and every step has a done-check (→ T2) ·
   the task decomposes into mechanical items with verifiable outcomes (→ T3).
5. **Cost sanity:** never pay T1 to watch tests run; never let T3 decide anything
   you can't cheaply undo.

## Trigger bindings

- **"fable plan" / "opus plan" {{TASK}}** → T1: produce a plan per 02-plan-handoff.
  No code edits in this phase. Deliverable: plan + executor contract.
- **"sonnet build" {{PLAN}}** → T2: execute plan steps in order under the executor
  contract; paste done-check output per step.
- **"haiku sweep: {{LIST}}"** → T3: mechanical items only; each item needs a stated
  done-check; anything ambiguous is returned to you, not attempted.
- **"adjudicate" {{DISAGREEMENT}}** → T1: read the actual code, rule on each
  disputed finding with quoted evidence, output final severity.

## Claude Code mechanics

- Switch session model with `/model`; use plan mode for T1 planning phases.
- Subagents can pin models (`model:` frontmatter, or the Agent tool `model`
  param) — run T3 workers under a T2 orchestrator instead of one big session.
- Background/batch anything a human doesn't need to watch; don't hold an
  expensive interactive session open for it.
