**Tier:** <Tiny | Small | Standard | High-risk> — <why> · **Brief:** docs/sdlc/changes/<slug>/change.md · **Ticket:** <link>

## Why
<One or two sentences.>

## What changed
<Behaviour, not files. "Cases now auto-close after 30 days instead of 60.">

## Blast radius
<What else could notice, and how it was checked.>

## Patterns followed
<Exemplars used. Any deviation and its decisions.md entry.>

## How it was verified
- Baseline: <N passed / M failed> → After: <N passed / M failed>
- Characterization tests committed before the change: <commit sha>
- Tests whose expectation changed ([WILL CHANGE]) and why:
- Repro test fails before / passes after (bugs): <yes / n/a>

## Rollback
<Command, flag, or revert. Call out any data step that cannot be reverted.>

## Known limitations

## Open questions and approvals still needed
<Unanswered questions with the default chosen; area-owner sign-off; map review.>

## Out of scope / follow-ups
