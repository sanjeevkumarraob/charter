# Change: <title>

Tier: <Tiny | Small | Standard | High-risk> — <one line why>
Ticket: <link> · Requested by: <name> · Area owner: <name>
Status: draft  <!-- draft → awaiting-approval → approved → built → merged | declined -->

## Why
<The problem in the requester's words. Why now.>

## Current behaviour
<What the code does today, with path:line references.>

Reproduction (bugs): `<command>` → <output seen> · matches the report: <yes / no>

## Desired behaviour
- When <trigger>, the system shall <response>.

## Out of scope
<Including tempting cleanups deliberately left for later.>

## Blast radius
| What could notice | Where | How we checked |
|-------------------|-------|----------------|
| Callers | | |
| Interfaces (API, CLI output, exit codes, exported functions, file formats) | | |
| Other services / event consumers | | |
| Data / reports | | |
| Jobs / schedules | | |
| Config / flags | | |
| History (why it is like this today) | | git log / blame / tickets, or "history unavailable" |

## Patterns to follow
<Exemplar files from docs/sdlc/codebase-map.md.>

## Proof
- Baseline before the change: <N passed, M failed, K skipped> (`<test command>`)
- Characterization tests added (current behaviour pinned):
- Tests marked [WILL CHANGE] and the new expectation:
- Repro test (bugs):
- New tests:

## Plan
| Step | Units / files | Proved by |
|------|---------------|-----------|
| 1 | | |

## Rollout and rollback
<Flag, migration order (expand → migrate → contract), how to undo. Standard and above.>

## Open questions
| Question | For whom | Default chosen | Answer |
|----------|----------|----------------|--------|

## Known limitations
<Things this change deliberately does not handle.>

## Follow-ups
<Drive-by fixes not done here.>

## Sign-off
<Who approved, when. High-risk: area owner required.>
