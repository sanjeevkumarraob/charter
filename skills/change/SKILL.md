---
name: change
description: Use when changing an existing codebase - a feature, enhancement, bug fix, refactor or migration in code that already exists, especially code someone else wrote. Sizes each change by risk, maps the codebase's established patterns as it goes, pins current behaviour with tests before touching it, traces the blast radius, and leaves a change brief and PR description reviewers can check the diff against. Keywords - brownfield, legacy code, existing codebase, change request, bug fix, enhancement, blast radius, characterization tests, patterns, PR description.
---

# Change

Goal: change existing code at AI speed without the three things teams fear most from
AI-written changes to code they own:
1. **Invented patterns** - new code that ignores how the codebase already does things.
2. **Unexplained code** - a diff nobody can justify during an incident six months later.
3. **Silent breakage** - behaviour changed somewhere the change never meant to touch.

The answers: a codebase map with named exemplar files, a short change brief written
before code, and tests that pin current behaviour before the change lands. Scale the
ceremony to the risk: a typo fix gets no paperwork, a schema change gets all of it.

Templates are in `templates/` next to this SKILL.md. For new apps, services or
prototypes with no existing code, use the `kickoff` skill instead.

## Where things live

| Artifact | Path |
|----------|------|
| Codebase map | `docs/sdlc/codebase-map.md` |
| Change brief | `docs/sdlc/changes/<YYYY-MM-DD>-<slug>/change.md` |
| PR description | `docs/sdlc/changes/<YYYY-MM-DD>-<slug>/pr.md`, also pasted into the PR |
| Decision log | `docs/sdlc/decisions.md` (append-only) |
| Area specs | `docs/specs/<area>.md` |

If kspec manages the repo, still use these paths and link `change.md` from the kspec
spec folder.

**The repo's own conventions win.** If it already has a decision log, roadmap, spec,
PR template, or a review gate written in `CLAUDE.md` / `AGENTS.md`, use those instead
of creating parallel files, and follow the repo's instructions wherever they differ
from this skill. For example, a repo-mandated adversarial review replaces the generic
one in Step 7, and an existing `DECISIONS.md` replaces `docs/sdlc/decisions.md`.

## When nobody can answer

Wherever this skill says ask or stop for approval and no person is available (headless
run, CI, the user stepped away): record the question and the default you chose under
**Open questions** in `change.md`, set `Status: awaiting-approval`, and do not go past
Step 5. Steps 1-5 change no behaviour, so they are safe to finish unattended.

## Detecting tools

- **kspec manages the repo** if `.kiro/steering/` or `.kiro/specs/` exists. Its commands
  that call an agent (`kspec analyse`, `kspec fix`, `kspec spec`) need `kiro-cli`; if it
  is not installed, follow the steps here instead.
- **A companion skill is available** if it appears in your list of skills. When one is
  missing, follow the step as written here. Never stop because a companion is missing.

## Step 1 - Triage

Before anything else, read just enough code to size the change, then classify it. State
the tier and why in one line. Pick the highest tier any signal points to. For a bug
report, reproduce it first (Step 2): you cannot size a bug you have not confirmed.

A **unit** is one command, endpoint, screen, job or event handler, plus its direct
helpers. In a monolith, judge by behaviour, not by file or folder.

| Tier | Typical signals | What it needs |
|------|-----------------|---------------|
| **Tiny** | Copy, typo, config value, dependency patch with no API change | Steps 2 and 8 only; baseline goes in `pr.md` |
| **Small** | Bug fix or contained change inside one unit; no contract, schema or data change | Light `change.md` (Why, Current, Desired, Proof), repro test for bugs |
| **Standard** | New or changed behaviour across units; a new endpoint, flag, screen, job or event (adding a new contract, versioned and documented) | Map, full `change.md` with plan, characterization tests |
| **High-risk** | Changes or removes an existing **public** contract (API, event, documented CLI output, the package's public exports, file format); changes names, paths or formats that existing stored data depends on; DB schema or data migration; auth; money; regulated or personal data; a shared library; another team's code; a danger zone in the map | Everything above, plus area-owner sign-off before build, rollout and rollback plan, adversarial review |

"Public" means something outside this codebase relies on it: the package entry point,
documented behaviour, or another service. A helper exported only so tests can reach it
is not a public contract.

If the user disagrees with the tier, use theirs and note it in `change.md`.

## Step 2 - Read before you write

- Find the code that implements the current behaviour. Cite it as `path:line`. For a
  bug, reproduce it and record the command and the output you saw.
- **If the behaviour is intended, cannot be reproduced, or is already fixed, stop.**
  Reply with the evidence (`path:line`, reproduction output, the commit or ticket that
  made it so) and change nothing. Write a `change.md` with `Status: declined` only if
  the request came from a ticket that needs a recorded answer. Mention any genuinely
  different problem you found as a separate suggestion, not part of this change.
- Trace everything that could notice the change: callers, other services, jobs,
  events, data and reports that read the same tables, and interfaces (CLI output,
  exit codes, exported functions, file formats).
- Check history on the lines you will touch: `git log -L <start>,<end>:<file>`, blame,
  linked tickets and PRs. Existing code often looks wrong for a reason; find the reason
  first. In a shallow clone, fetch a bounded depth (`git fetch --depth=200`); unshallow
  fully only if that does not reach where the code came from. If history cannot be
  fetched, record "history unavailable".
- Run the existing tests. Record the suite's pass/fail counts as the **baseline** in
  `change.md` (in `pr.md` for Tiny).

Unknowns become questions for the area owner (CODEOWNERS or the map), not guesses.

## Step 3 - Codebase map (Standard and High-risk)

Read `docs/sdlc/codebase-map.md` if it exists. Refresh it if it is more than 90 days
old or does not cover the area you are touching.

If it is missing, start it from `templates/codebase-map.md`, covering **only the
patterns and area this change touches**. The map grows one change at a time; do not map
the whole repo up front.

- For each pattern you touch, find 1-2 files that do it well and record them as the
  **exemplar**: "To add a CLI flag, follow `src/commands/sync.js`." This is the main
  defence against invented patterns. Delete template rows that do not apply and add
  rows for this repo's own units of change.
- Record danger zones you find: generated code, frozen or legacy code, code other teams
  depend on, code with no tests.
- Tag each section `observed` (read in code) or `confirmed` (a person agreed). Note
  individual exceptions only.
- Do not block on review, but list "map review by an engineer on the owning team" under
  approvals still needed. Their review turns the map from "what the AI thinks" into the
  team's own reference, and makes them co-owners of how agents work in their code.

`CLAUDE.md` / `AGENTS.md`: if they exist, add one line pointing at the map. If neither
exists, create a minimal `AGENTS.md` with the build and test commands and that pointer.
Do not copy the map into them.

If kspec manages the repo and `kiro-cli` is installed, run `kspec analyse` and build the
map from its steering docs. If `context-engineering` is available, use it to decide what
goes in the map versus `AGENTS.md`.

## Step 4 - Change brief (`change.md`)

Skip for Tiny. Write it from `templates/change.md`. Ask one or two questions at a time
to fill gaps, and dig for why this is needed now, not just what. Required: Why, Current
behaviour, Desired behaviour, Out of scope, Blast radius, Patterns to follow, Proof,
Plan, and (Standard and above) Rollout and rollback.

The **Plan** section lists the units and files that change, the order of work, and the
test that proves each step. Make the plan in plan mode or the agent's read-only mode.

Ask for approval, and continue through Step 5 while you wait, never past it: pinning
behaviour changes nothing and often turns up facts that change the brief. High-risk approval must come
from the owner of the affected area, not only the requester.

## Step 5 - Pin current behaviour

- **Bug**: write a test that reproduces the bug and fails for the reason in `change.md`.
  If `debugging-and-error-recovery` is available, use it to find the root cause.
- **Standard and High-risk**: wherever the behaviours you will touch, or behaviours next
  to them, are not already pinned by a test, add characterization tests that record
  what the code does today, even where today's behaviour looks odd.
- Put `[WILL CHANGE]` in the name of every test whose expectation this change is meant
  to alter, and list those tests under Proof.
- Commit these tests on their own, before any behaviour change, so reviewers can see
  exactly which expectations the change alters.

**Stop here** until `change.md` is approved, or with `Status: awaiting-approval` if
nobody can answer.

## Step 6 - Build the smallest change

- Follow the exemplar for every pattern you touch. Match its naming, structure, error
  handling, logging and test style, even where you would do it differently.
- No drive-by refactors. List them under Follow-ups and do them in a separate change.
- No new dependency, framework or pattern without a line in `docs/sdlc/decisions.md`:
  `- YYYY-MM-DD · <what> · decided by <who> · why: <reason> · <link>`. If a new pattern
  is genuinely needed, add it to the map in the same PR.
- Contract, schema or data changes: expand, migrate, then contract, so each step can
  roll back on its own. If `deprecation-and-migration` is available, follow it.
- Work in thin slices and run the tests after each one. Use `incremental-implementation`
  and `test-driven-development` when available.
- If kspec manages the repo and `kiro-cli` is installed: `kspec fix` for bugs,
  `kspec refactor` for refactors, `kspec spec` for Standard and High-risk work, with
  `change.md` as the input.
- If the implementation departs from the Plan, update the Plan in the same commit.

## Step 7 - Verify

- Run the full suite and compare it with the baseline. Only the `[WILL CHANGE]` tests
  may change expectation; say which changed and why.
- Never edit any other existing test to make it pass.
- High-risk: if `doubt-driven-development` is available, run it on the final diff.
  Otherwise ask a fresh agent, with no conversation history, to review the diff against
  `change.md` and list anything that does not trace to it.
- If `code-review-and-quality` is available, run it before opening the PR.

## Step 8 - PR description

Write `pr.md` from `templates/pr-description.md` and paste it into the PR. For test
output, include the summary lines (counts, before and after) and the names of changed
tests, not the full log. A reviewer should be able to judge intent and risk from the
description and then check the diff against it, rather than reading every line to work
out what the change is for.

## Step 9 - Leave the codebase easier to change

- Update the map with anything you learned: a new exemplar, a danger zone, a flaky test.
- **Spec what you touch.** If the area has no spec, add or extend `docs/specs/<area>.md`
  with the behaviour you just confirmed. The legacy system gains specs one changed area
  at a time, without a big documentation project.
- If the area's spec changed, kspec is installed and the spec is linked to Jira, run
  `kspec drift` so product sees the change on the ticket.

## Finish

Report: tier and why; files changed; tests added (characterization, repro, new); test
counts against the baseline; open questions with the defaults you chose; follow-ups;
approvals still needed (brief, area owner for High-risk, map review). Do not merge or
deploy; that stays with the people who own the repo.

`change.md` status values: `draft` → `awaiting-approval` → `approved` → `built` →
`merged`, or `declined` when Step 2 shows no change is needed.

## Companion skills

From `addyosmani/agent-skills` (`npx skills add addyosmani/agent-skills`).

| Skill | Used in |
|-------|---------|
| `context-engineering` | Step 3 map and agent context |
| `debugging-and-error-recovery` | Step 5 bugs |
| `deprecation-and-migration` | Step 6 contract, schema and data changes |
| `incremental-implementation`, `test-driven-development` | Step 6 build |
| `git-workflow-and-versioning` | Separate commits for pinned tests and the change |
| `doubt-driven-development`, `code-review-and-quality` | Step 7 verify |
| `documentation-and-adrs` | Decisions that change architecture |

If the repo has a `CONSTRAINTS.md` (from `constraint-driven-development`), treat it as a
hard bar for every tier.
