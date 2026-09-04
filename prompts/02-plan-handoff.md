# Plan → Cheap-Executor Handoff

**Use when:** a top-tier model ("fable plan" / "opus plan") plans and a cheaper tier
executes. The plan format + executor contract below is what makes that safe.

**What it guards against:** cheap-model silent improvisation — the executor hits an
unexpected state, guesses, and buries the guess mid-transcript. Stop conditions and
per-step done-checks convert improvisation into a report-back.

## Planner prompt (T1)

```text
Produce an execution plan for: {{TASK}}

Rules for the plan itself:
- Assume the executor is competent but literal, with no context beyond this plan
  and the repo. It will not infer intent — encode intent.
- Read the relevant code before planning; cite the files you read.

Output exactly this structure:

OBJECTIVE — one sentence.
DONE MEANS — observable criteria (commands + expected results), not vibes.
INVARIANTS — things that must NOT change (public APIs, behavior, specific
  files). Explicit list.
CONTEXT — files the executor must read before step 1, one line each on why.
STEPS — numbered. Each step has:
  intent: what this step accomplishes and why
  files: exact paths
  change: precise description (or the diff itself if short)
  done-check: command to run + expected output
  risk: what could go wrong here + what that would look like
STOP CONDITIONS — concrete situations where the executor must halt and report
  (e.g. "done-check for step 3 fails", "file X does not exist", "any test not
  named in this plan starts failing").
OUT OF SCOPE — tempting-but-forbidden changes.
```

## Executor contract (prepend to the T2/T3 execution prompt)

```text
Execute the plan below under this contract:
1. Read every file in CONTEXT before starting step 1.
2. Execute steps in order. After each step, run its done-check and paste the
   real output. A step without passing done-check output is NOT done.
3. Zero improvisation: if reality differs from the plan in any way — missing
   file, different signature, failing precondition, ambiguity — STOP. Report
   the step number, what you expected, what you found (with evidence). Do not
   guess and continue.
4. Any STOP CONDITION triggers → halt immediately, report, wait. On any halt,
   leave the working tree untouched for inspection — do not revert or clean up.
5. Never touch anything under INVARIANTS or OUT OF SCOPE, even if it looks
   broken. Note it under "Noticed, not changed" instead.
6. Final report: table of step → status (done / stopped / skipped) →
   done-check evidence. Failures reported as failures.
```

## Why it works

The expensive reasoning is front-loaded into the decisions a cheap model can't
make well (what to change, what must not change, what "working" looks like), and
the cheap model is left with labor it can do (apply precise changes, run checks,
report). Quality is enforced by the checks, not by trusting the executor.
