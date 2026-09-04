# Learnings Capture (post-run / post-incident)

**Trigger:** "capture learnings" — after a big run, migration, launch, or incident
**Routing:** T2 drafts from the session; you confirm KEEP/CHANGE/STOP; T3 can file
the follow-ups.
**Why this template (from your own flow):** you already write run-learnings docs
(e.g. the aih 2.7.0 enterprise run). This makes them 15 minutes, uniform, and —
the part that usually gets lost — converts each learning into a repo/canon edit
instead of a note that never changes behavior.

## Prompt

```text
Capture learnings from: {{RUN — what was attempted, when, where}}
Sources: this session, plus {{LOGS / NOTES / PATHS}}.
Write the result to: {{DEST_PATH — e.g. docs/learnings/<date>-<run>.md}}

Fill exactly this structure (target: 15 minutes, one page):

# Learnings: {{run-name}} — {{date}}

## What was attempted
2-3 sentences, with the goal as originally stated.

## What actually happened
Outcome vs expectation. Deltas only — no play-by-play.

## Surprises
Things that violated an assumption. For each: the assumption, what reality
showed, evidence pointer.

## Friction hit
Concrete time-losers (this feeds the friction register — see
22-friction-extraction). Each with a rough cost.

## Keep / Change / Stop
KEEP: what worked and should become standard.
CHANGE: what to do differently next time — specifically, not "be more careful".
STOP: what to not do again.

## Canon conversion (MANDATORY — a learning that doesn't land in the repo
didn't happen)
For every KEEP/CHANGE/STOP item, name WHERE it lives: which repo file, rule,
checklist, or template gets edited, and the one-line edit. If the answer is
"nowhere", say why it is genuinely one-time.

## Follow-ups
action · owner · date. No owner + date = delete the item now instead of
letting it rot.
```
