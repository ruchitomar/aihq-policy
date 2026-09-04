# Documentation Alignment

**Trigger:** "align docs"
**Routing:** T3/T2 extract and verify claims; T2 writes the fixes; anything
ambiguous gets asked, not guessed.
**What weak doc passes skip:** verification. They rewrite prose to sound fresher
while copying stale facts forward. The claims-table method makes every fix trace
to a source of truth.

## Prompt

```text
Align documentation with reality for {{SCOPE — repo / docs paths}}.
Docs in scope: {{e.g. README.md, docs/, CLAUDE.md, setup guides}}.

STAGE 1 — EXTRACT CLAIMS
Read each doc and extract every TESTABLE claim into a table: commands, versions,
paths, env vars, API signatures, config keys, port numbers, feature statements
("supports X"). Prose opinions are not claims; skip them.

STAGE 2 — VERIFY each claim against its source of truth, one row at a time:
- command → run it only if read-only/idempotent; verify stateful or destructive
  commands (installs, deploys, deletes) via --help, dry-run, or reading the
  code — never by executing them
- path / file → check existence
- version → package manifest / lockfile
- API / signature → read the code
- env var / config key → grep the code for where it is actually read
Status per claim: MATCH · DRIFT (doc says X, reality is Y — cite file:line or
command output) · UNVERIFIABLE (say why).

STAGE 3 — FIX, in this order:
1. Safety-relevant drift (wrong destructive command, wrong credential handling)
2. Onboarding-blocking drift (setup steps that fail)
3. Everything else
Rules: every edit cites its source-of-truth evidence · never delete a claim you
could not verify — mark it `<!-- UNVERIFIED: {{why}} -->` and list it · never
"improve" prose in a way that introduces new unverified claims.

STAGE 4 — REPORT
The claims table with statuses · edits made · the UNVERIFIABLE list for human
review · any doc-vs-doc contradictions found (two docs disagreeing — flag them,
do not silently pick a winner).
```
