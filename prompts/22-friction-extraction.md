# Friction Extraction (what's hurting progress)

**Trigger:** "extract friction"
**Routing:** T3/T2 gather evidence; T2 clusters; you confirm the top 3 —
countermeasures change how you work, and that is a human call.
**What weak analysis skips:** separating annoyance from throughput loss, requiring
repeated evidence before naming a pattern, and attaching a cost so the register
self-prioritizes.

## Prompt

```text
Find the patterns that are hurting progress in {{SCOPE — repo(s), or "my recent work"}}.
Evidence sources: git history, CI history, session/learnings notes at
{{NOTES_PATHS — e.g. D:\dev\*learnings*.md}}, TODO/FIXME density, and anything
else measurable.

STAGE 1 — GATHER (run these; paste summaries, not dumps)
- Churn: git log --since="{{PERIOD=8 weeks}}" --format= --name-only | sort | uniq -c | sort -rn | head -20
  (files edited again and again = rework magnet or hot spot)
- Rework signals: git log --since="{{PERIOD}}" --oneline | grep -iE "revert|fixup|hotfix|typo|again"
- Stale branches: git branch -a --sort=committerdate, with last-commit age
- CI red rate + retry rate (deep dive → the ci-pattern-analysis template)
- Learnings/session notes: recurring complaints, repeated manual steps
- TODO/FIXME: count, plus age via git blame on a sample

STAGE 2 — CLUSTER into named friction patterns. A pattern requires ≥3
independent instances — below that it goes on a "watch" list, not the register.
For each pattern:
  signal (the evidence, cited) · mechanism (why it costs time) · cost estimate
  (hours/week, conservative) · root cause · cheapest countermeasure that
  removes the cause (not a coping ritual) · leading indicator that will show
  the fix worked.

STAGE 3 — SEPARATE
- Throughput losses (block or redo work) vs annoyances (feel bad, cost little).
  Annoyances rank at the bottom no matter how irritating.
- One-time events vs recurring patterns. One-time → a learnings note, not the
  register.

STAGE 4 — OUTPUT
Friction register sorted by cost · top 3 countermeasures with a first concrete
step each · the watch list. Every register entry cites its evidence — an
uncited pattern is an opinion.
```
