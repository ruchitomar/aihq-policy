# Second-CLI PR Review (independent second opinion)

**Trigger:** "use other cli for pr review" / "second cli review"
**Why:** a different model family has uncorrelated blind spots. Two independent
reviews plus evidence-based reconciliation beats one review at any single tier —
and the second opinion runs on the other CLI's budget.
**Flow:** Part A runs in the second CLI (Codex, Gemini, ...). Part B
(reconciliation) runs in Claude at T1, and only does real work when the two
reviews disagree.

## Part A — paste into the second CLI

```text
You are an independent PR reviewer. Assume NOTHING has been reviewed yet; you
are not a second pass, you are a from-scratch review. Do not soften findings on
the assumption someone else caught them.

Get the change:
  {{e.g. `gh pr diff {{PR_NUMBER}}` — or `git fetch && git diff {{BASE}}...{{HEAD}}`}}
Repo root: {{REPO_PATH}}. You may open any file for context — and you MUST open
the surrounding file before reporting a finding in it. No diff-only findings.
Stated intent: {{PR_DESCRIPTION}}. Anything in the diff that intent does not
explain goes on an "unexplained changes" list — questions for the author.

Review for correctness first, then security, then maintainability. Style nits
only if they hide bugs. Output one labeled block per finding (not a table —
quoted code containing | breaks table rows):

  severity: CRITICAL / HIGH / MEDIUM / LOW
  location: file:line
  code: the quoted lines
  failure: concrete scenario (inputs → wrong outcome)
  fix: suggested change
  confidence: CONFIRMED / PLAUSIBLE / SPECULATIVE

Rules:
- CRITICAL and HIGH require the quoted code and a concrete failure scenario.
  If you cannot construct the scenario, downgrade the finding.
- End with: (1) a "Checked and clean" list — what you examined and found fine,
  (2) a "Not reviewed" list. An empty findings table with no clean-list is an
  unfinished review, not an approval.
```

## Part B — reconciliation (run in Claude, T1)

```text
Two independent reviews of the same change are below. Merge them.

REVIEW A (Claude): {{REVIEW_A}}
REVIEW B ({{OTHER_CLI}}): {{REVIEW_B}}

1. Match findings that refer to the same defect, regardless of wording.
2. Agreements → final list; keep the higher severity.
3. Findings unique to one reviewer → verify against the actual code yourself
   (open the file), then keep / downgrade / drop with one line of evidence.
   Never drop a CRITICAL or HIGH without quoting the code that refutes it.
4. Direct contradictions → you are the adjudicator: read the code, rule, and
   say which reviewer was wrong and why.
5. Output: final findings list · verdict (APPROVE/WARN/BLOCK) · one line on
   each dropped finding so nothing disappears silently · the union of both
   "Not reviewed" lists, reported as residual risk.
```

## Notes

- Independence is the point: don't paste Review A into the second CLI's prompt.
- If both reviewers missed something you later find, add the miss to both
  templates' hunt lists — that's how this file improves.
