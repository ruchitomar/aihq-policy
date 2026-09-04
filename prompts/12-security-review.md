# Security Review

**Trigger:** "security review"
**Routing:** T1 for the threat model and adjudication; T2 may run the class sweep;
never T3-only.
**What weak reviews skip:** the threat model (they grep for `eval(` and call it a
day), concrete attack paths, and "absence claims" — stating what was checked and
found clean, so silence is distinguishable from unexamined.

## Prompt

```text
Security review of: {{SCOPE — diff, module, or repo path}}
Context: {{WHAT_IT_DOES, WHO_USES_IT, WHERE_IT_RUNS}}

STAGE 1 — THREAT MODEL (before reading a line of code)
- Assets: what is worth stealing or corrupting here (data, tokens, compute, trust)?
- Entry points: every place external input enters (HTTP, CLI args, env, files,
  webhooks, MCP/tool calls, DB reads of user-written data).
- Trust boundaries: where does data cross from less-trusted to more-trusted?
Enumerate these as lists. The sweep below walks the entry points, not vibes.

STAGE 2 — CLASS SWEEP (walk each entry point through each class; per class
report findings or "checked, clean — here is what I looked at")
- Injection: SQL/NoSQL/command/template — any query or command built by
  concatenation or interpolation from tainted data?
- AuthN/AuthZ: endpoints missing checks; IDOR (user A reaching user B's
  object); privilege checks done client-side or after the action.
- Secrets: hardcoded keys/tokens/passwords; secrets in logs, error messages,
  URLs, or committed config. If in scope, check `git log -p` for recently
  removed ones — removal is not rotation.
- SSRF: any fetch/request to a URL influenced by input.
- Open redirect: any redirect target influenced by input.
- Path traversal: any filesystem path built from input.
- Races / TOCTOU: check-then-use gaps on files, permissions, or balances.
- Deserialization / dynamic eval of untrusted data.
- XSS/CSRF (web surfaces): unescaped output, missing CSRF on state changes.
- Crypto: home-rolled anything, ECB, static IVs, MD5/SHA1 for security
  purposes, disabled cert validation.
- Sensitive data handling: PII in logs, over-detailed error messages, missing
  rate limits on auth or expensive endpoints.
- Dependencies: known-vulnerable versions, install scripts, typosquat-looking
  names.

STAGE 3 — FINDINGS
Each: severity · file:line · quoted code · ATTACK PATH (concrete: attacker does
X with input Y → outcome Z) · impact · fix · confidence. No attack path → not a
finding; put it under "hardening suggestions" instead. This kills checklist spam.

STAGE 4 — REPORT
- Findings table sorted by severity.
- Absence claims: classes checked and clean, with what was examined.
- If a CRITICAL involves a live exposed secret: flag ROTATE FIRST — rotation
  before any code fix, and removal from history is not rotation.
```
