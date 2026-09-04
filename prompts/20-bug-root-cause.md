# Bug Root Cause

**Trigger:** "root cause {{BUG}}" — any debugging beyond the trivial
**Routing:** T2 reproduces and gathers evidence; T1 when hypotheses survive two
rounds. Escalation is mandatory at two failed fixes, not optional.
**What weak debugging skips:** reproducing before theorizing, discriminating
evidence between hypotheses, and the mechanism. Cheap models shotgun plausible
fixes until the symptom hides — which is not the same as the bug being fixed.

## Prompt

```text
Bug: {{SYMPTOM — exact error / wrong behavior, and the command or steps that show it}}

PHASE 1 — REPRODUCE (do not skip to theories)
Run it. Paste the actual failure output. If you cannot reproduce, THAT is the
task now: find the environment/data/timing difference. No fix attempts before a
reproduction, or an explicit "cannot reproduce because X".

PHASE 2 — TIMELINE
When did this last work? Check git log for the touched area; identify candidate
commits. If the repro is cheap, bisect: `git bisect start` / `bad` / `good`.

PHASE 3 — HYPOTHESES (before changing anything)
List 2-4 hypotheses ranked by likelihood. For EACH, state the discriminating
evidence: "if H1, the log/state shows X; if H2, X is absent and Y appears
instead." Then gather that evidence — targeted logging, state inspection, a
probe test. Kill hypotheses with evidence, not preference. Paste the evidence.

PHASE 4 — FIX
State the mechanism in one sentence: "{{cause}} produces {{symptom}} because
{{chain}}." The fix targets the cause. Forbidden: shotgun changes, blanket
try/catch to hide the symptom, and "upgraded dependencies and it went away"
without knowing why.

PHASE 5 — PROVE IT
- Regression test that FAILS without the fix and PASSES with it. Show both runs.
- Re-run the original reproduction. Paste the output.
- One-paragraph writeup: mechanism, why existing tests missed it, and where
  else the same pattern exists — grep for it; the same bug often has siblings.

STOPPING RULE
Two fix attempts failed → stop. Output: the reproduction, evidence gathered,
each hypothesis with status (killed / alive + why), and the next probe you
would run. That summary is the escalation handoff for a stronger model.
```
