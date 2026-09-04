# Design Doc (design.md)

**Trigger:** "design doc {{FEATURE / PROJECT}}"
**Routing:** T1 writes Options, Decision, and Failure Modes; T2 fills the mechanical
sections; T1 re-reads the finished doc once.
**What weak design docs skip:** non-goals, honest alternatives (one real option plus
two strawmen), failure modes, and rollback. Those four sections are where the
reasoning actually lives — everything else is transcription.

## Prompt

```text
Write design.md for: {{WHAT}}
Inputs: {{REQUIREMENTS / LINKS / CONSTRAINTS}}. If a repo exists, read the
current code first; cite what you read.

Structure (all sections mandatory; "N/A" requires one line of why; sections may
be one line each — brevity is fine, omission is not):

# Design: {{name}}
Status: draft · Date · Author · Reviewers

## 1. Context & Problem
What hurts today, with evidence (metrics, incidents, user quotes).

## 2. Goals
Observable outcomes. Each goal testable: how would we know it is met?

## 3. Non-Goals
Explicitly out of scope, one line of WHY each. This is the scope-creep fence.

## 4. Constraints
Hard constraints (deadline, compliance, platform, budget) listed separately
from preferences. Do not mix them.

## 5. Current State
How it works today, from reading the code — with file pointers, not memory.

## 6. Options (≥2 real options)
QUALITY BAR: each option written so its strongest proponent would call the
summary fair. Per option: approach, what it is best at, real costs and risks,
rough effort. No strawmen — if one option is obviously dumb, find a better
second option; the comparison is worthless otherwise.

## 7. Decision
Which option, and the deciding factors — which constraint or goal tipped it.
Also: what new information would reverse this decision.

## 8. Architecture
Components, data flow, contracts/interfaces, storage. Text diagrams (mermaid)
are fine.

## 9. Failure Modes
What breaks first under: 10x load · malformed or hostile input · dependency
outage · partial deploy. For each: blast radius + mitigation.

## 10. Security & Privacy
Trust boundaries, authN/Z, data classification, secret handling. If this
touches auth, payments, or PII: run the security-review template on the design
itself before implementation.

## 11. Testing Strategy
How correctness gets proven per layer; which behaviors only show up in
integration/E2E and how those are covered.

## 12. Rollout & Rollback
Stages, flags, migration order — and the concrete rollback procedure: "how do
we get back to the previous state in 10 minutes."

## 13. Open Questions
Each with an owner and a resolve-by date. An open question without an owner is
a future incident.

## 14. Success Metrics
How we will know it worked, and where that is measured.

Honesty rules: unknowns stay labeled unknown — no papering over. Every factual
claim about the current system carries a file pointer. Estimates are ranges,
not points.
```
