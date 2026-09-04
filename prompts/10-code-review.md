# Code Review

**Trigger:** "code review" (working diff, branch, or PR)
**Routing:** T2 default · T1 when the diff touches auth, payments, money, migrations,
concurrency, crypto, or a public API · T3 findings are input, never verdicts.
**Pairs with (Claude Code):** the /code-review skill and code-reviewer agent. This
template is the standalone version for any CLI.

**What weak reviews skip:** tracing data flow (they pattern-match style instead),
checking callers of changed signatures, error paths, whether tests assert behavior
or restate the implementation, and whether the diff matches the stated intent.

## Prompt

```text
Review this change: {{DIFF_SOURCE — e.g. `git diff main...HEAD` or `gh pr diff {{N}}`}}
Stated intent: {{WHAT_THE_CHANGE_CLAIMS_TO_DO}}

STAGE 1 — INTENT
Restate what this change claims to do in one sentence. Anything in the diff not
explained by that intent goes on an "unexplained changes" list — each item is a
question for the author, not silently accepted.

STAGE 2 — MAP
List changed files with one line each: role, blast radius (who calls this / who
depends on it). For every changed function signature or contract, search for
callers and check each call site. Paste the search you ran.

STAGE 3 — CORRECTNESS HUNTS (do each one; report findings or "clean")
- Data flow: for each changed function, trace inputs → outputs including the
  unhappy path. Where does a null / empty / oversized / malformed input end up?
- Error paths: every new call that can fail — who catches it, is it swallowed?
- Resource lifecycle: opens without closes, locks without unlocks, listeners
  without teardown, transactions without commit/rollback.
- Concurrency: shared state touched? reentrancy? await/async gaps where state
  mutates underneath?
- Boundaries: off-by-one, first/last element, empty collection, zero, negative.
- Tests: do new/changed tests assert observable behavior, or mirror the
  implementation? Would they fail if the code were wrong in the obvious way?
  If the suite runs cheaply, run it and paste the summary tail — otherwise
  "tests not run" goes on the Stage 5 list.
- Security quick pass: input handling, query building, path construction,
  secrets in code or logs. (Anything real → run the security-review template.)

STAGE 4 — FINDINGS
Each finding: severity (CRITICAL/HIGH/MEDIUM/LOW) · file:line · the code quoted ·
concrete failure scenario (inputs/state → wrong outcome) · suggested fix ·
confidence (CONFIRMED/PLAUSIBLE). A CRITICAL or HIGH without a failure scenario
is not a finding — downgrade it, or do the work to construct the scenario.

STAGE 5 — VERDICT + HONESTY
- Verdict: APPROVE (no CRITICAL/HIGH) / WARN (HIGH only) / BLOCK (any CRITICAL).
- "Not reviewed" list: what you did NOT check (files only skimmed, tests not
  run, generated code). Silence is a claim — don't make it accidentally.
```
