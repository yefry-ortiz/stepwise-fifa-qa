# Working-Set Protocol (multi-turn refinements)

Most refinements are a single turn — read the target artifact, make one surgical
edit, respond. That path needs no working set and should stay as lean as it is.

This protocol applies ONLY when a refinement session runs for several turns (the
human iterates: "now also fix…", "actually change…"). Each operator turn is
served by a freshly spawned executor process; without a carried-forward note the
agent re-reads the same artifacts and upstream sources every turn, and each
re-read inflates the context re-sent on all later turns. The note replaces
re-reading with recall.

## The note file

- Path: `REFINEMENT-NOTES.md` at the sandbox root (the
  `STEPWISE_REFINEMENT_SANDBOX_ROOT` / `STEPWISE_REFINEMENT_NOTES` location),
  **outside the writable target artifacts**.
- It is therefore NOT part of the diff Stepwise applies back. It is session
  memory, not a deliverable.
- Keep it terse and current. Overwrite stale content; do not grow a transcript.

## Schema

```
# Refinement Notes

## Request thread
- turn N: <one-line summary of what was asked + what was changed>

## Targets touched
- <sandbox target path> — <section/IDs edited> [turn N]

## Evidence map
- <read-only source path> — <the fact it established> [read@turn N]

## Open / pending
- <unresolved item> — <missing evidence>
```

## Turn discipline

### Turn 0 (first request)

Proceed normally (SKILL.md Steps 1–6). If the session looks like it will iterate,
seed `REFINEMENT-NOTES.md` with the targets touched and the evidence used so the
next turn can recall instead of re-read.

### Resume turns (second and later requests)

1. **Read `REFINEMENT-NOTES.md` first** — the cheapest orientation.
2. Do NOT re-read the full target artifact or re-search upstream sources that the
   Evidence map already covers. Re-read a source only if this request implicates
   a section the notes do not cover, or it changed.
3. After editing, update the note (request thread, targets touched, evidence,
   open items) so the next turn inherits the progress.

## Worth-a-session check

Before opening or continuing a sandbox session for a trivial cosmetic change
(e.g. a one-word title tweak), consider whether the edit is worth a full
turn — a discarded cosmetic refinement spends a turn for nothing. State the
change, make it in one surgical edit, and avoid exploratory reads.
