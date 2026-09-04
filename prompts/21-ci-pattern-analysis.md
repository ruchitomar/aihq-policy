# CI Pattern Analysis

**Trigger:** "analyze ci"
**Routing:** T3 gathers, T2 clusters and hypothesizes, T1 only if the top fix is
architectural.
**What weak analysis skips:** evidence thresholds (one red run becomes "flaky"),
separating failure classes, and pricing the problems so fixes are ranked by payback
instead of by annoyance.

## Prompt

```text
Analyze CI for {{REPO}} over the last {{N=50}} runs.

STAGE 1 — GATHER (paste the commands you ran; summarize, don't dump)
  gh run list --limit {{N}} --json databaseId,conclusion,name,workflowName,event,createdAt,updatedAt,headBranch
  For each failed run: gh run view <id> --log-failed
    → extract the failing step + a short error signature per failure.
Also collect durations per workflow/job for the cost view.

STAGE 2 — CLASSIFY each failure into exactly one:
  regression (real code break) · flaky-test (intermittent, same test, unrelated
  commits) · infra (runner/network/service) · dependency (external version or
  registry) · timeout · config/cache. "Unclassified" is allowed — never force a
  class without evidence.

EVIDENCE THRESHOLDS
- "flaky" requires ≥2 failures of the same test across unrelated commits, or a
  fail→retry-pass on the same commit. One failure is "unclassified", not flaky.
- Every root-cause hypothesis names its discriminating evidence — "if true, the
  log shows X" — and whether X was actually found.
- Check workflow-config history inside the window (git log on the CI config
  paths); a pattern straddling a config change is suspect — split the window.

STAGE 3 — COST VIEW
- Slowest 5 jobs (typical and worst-case duration) and their share of total CI
  minutes.
- Repeated work: uncached dependency installs, redundant matrix entries, jobs
  that always pass and gate nothing.
- Queue-vs-run time, if visible.

STAGE 4 — OUTPUT
1. Pattern table: pattern · frequency · evidence (run ids) · root-cause
   hypothesis + confidence · proposed fix · expected saving (minutes/week or
   red-runs/week).
2. Top 3 fixes ranked by (saving × frequency) / effort — each with its first
   concrete step.
3. Watchlist: what to re-measure after the fixes to confirm they worked.
```
