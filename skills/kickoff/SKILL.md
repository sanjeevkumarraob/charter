---
name: kickoff
description: Use before starting something new - a new app, service, prototype or bridge tool, at work or on a personal project - to set up an AI-native SDLC trail before code is written - intent, spec, plan, known gaps, agent context (CLAUDE.md, AGENTS.md), review rules and a runbook. For changes to an existing codebase use the change skill instead. Keywords - kickoff, greenfield, new project, intent.md, spec-driven, plan mode, handover, known gaps, vibe coding, AI SDLC.
---

# Kickoff

Goal: before any code is written, produce a committed artifact trail that makes AI-built
work traceable, reviewable and supportable by someone who was not in the room. The trail
answers "it's vibe coded" with evidence: who asked for what, why, what was decided, what
was built, and what is still deliberately unfinished.

Work through the gates in order. Each gate ends with a file and a pause for human
approval. Never skip a gate silently; if the user skips one, record it in `known-gaps.md`.
Templates for every file are in `templates/` next to this SKILL.md.

This skill is the intent, ownership and handover layer. For build-quality work it
delegates to companion skills when they are installed (see "Companion skills"); check
which ones are available in Gate 0 and fall back to the built-in steps for any that are
missing. Never copy a companion skill's content into this one.

## Gate 0 - Detect context

0. If the request is a change to code that already exists (a feature, fix or refactor
   in an existing repo), stop and use the `change` skill instead. `kickoff` is for new
   apps, services, prototypes and bridge tools.

1. Look for existing structure: `CLAUDE.md`, `AGENTS.md`, `.kiro/`, `specs/`, `docs/`,
   ADR folders, `.github/`. Reuse what exists; never add a parallel structure.
2. Choose the artifact home:
   - `.kiro/` present and kspec installed -> write `intent.md` and `known-gaps.md` into the
     active kspec spec folder and let kspec own spec/design/tasks (see "Working with kspec").
   - Otherwise -> `docs/sdlc/<kebab-slug>/`.
3. Note which companion skills are available in this session.
4. Ask one question: is this work (shared ownership, regulated data, other teams involved)
   or personal? Work defaults to the full trail. Personal may use the light path:
   intent -> plan -> known gaps -> agent context.

## Gate 1 - Intent (`intent.md`)

Interview before writing. Ask one or two questions at a time. People's first framing often
stops midway, so dig for the motive behind the request: what triggered it now, who is
hurting, what happens if nothing is built.

Cover:
- Problem in plain words, and the evidence (shadowing notes, tickets, incidents, metrics)
- Who is affected, and who owns the problem on the business side
- Business impact with numbers where possible (backlog, churn, hours, cost)
- Constraints: data classification, compliance, systems that must not be touched,
  overlapping team roadmaps
- Ownership: who builds, who runs it, who gets paged at 2am
- Lifespan: permanent, or a bridge with a sunset condition (the event that retires it)
- Out of scope, open questions

If `interview-me` is available, use it to run this interview, then map its findings onto
the sections above. Otherwise interview as described.

Write `intent.md` from `templates/intent.md`. Ask the user to have the business owner
review it. Stop for approval.

## Gate 2 - Spec (`spec.md`)

From the approved intent, write requirements and design using `templates/spec.md`. If
`spec-driven-development` is available, follow its method to draft the requirements, then
make sure every requirement traces to `intent.md` and the Concerns section is filled. Apply
any security, compliance or UX skills available in the session. Requirements must be
testable; use "When <trigger>, the system shall <response>" where it helps. Always fill
the Concerns section: policy conflicts, overlap with another team's roadmap, data risks.
Stop for approval.

## Gate 3 - Plan (`plan.md`)

Plan without editing files (use plan mode or the agent's read-only mode if it has one).
If `planning-and-task-breakdown` is available, use it to split the work into small,
independently testable tasks.
Fill `templates/plan.md`: files that change, order of work, the test that proves each
step, risks, alternatives rejected. Interrogate it: what could this break, which step is
riskiest. Iterate until an engineer who never saw this conversation could implement from
the plan alone. Stop for approval.

## Gate 4 - Known gaps (`known-gaps.md`)

Fill `templates/known-gaps.md` honestly: what is deliberately prototype-grade or not yet
done (auth, error handling, retention, monitoring, load, accessibility, coverage, skipped
gates). For each gap, say what production-ready requires and who sets that bar. This is
the checklist other teams can hold the work to; naming gaps first disarms "it's not
proper".

Set the quality bar with the people who will judge the work. If
`constraint-driven-development` is available, run it with them (at work, the owning
engineering team, not the builder) to record the bar in `CONSTRAINTS.md`; that skill then
watches diffs for anyone quietly lowering it. Link each known gap to the constraint that
closes it. Without that skill, ask the owning team for their bar and record it in the
"Production-ready means" column.

## Gate 5 - Agent and review context

Create or update, each under about a page:
- `CLAUDE.md` - commands with healthy output, conventions, architecture, "things the agent
  gets wrong", and a Verifying section (run build, tests, lint before reporting done;
  fix code, not tests).
- `AGENTS.md` - the same essentials, tool-neutral, for Kiro, Copilot, Cursor, Codex and
  others. If the repo prefers one source, make one file point at the other.
- `REVIEW.md` - from `templates/REVIEW.md`: review passes, what counts as Important,
  and a rule to flag diff content not traceable to `plan.md`. If `code-review-and-quality`
  is available, name it in `REVIEW.md` as the review method and keep `REVIEW.md` for the
  repo-specific rules only.
- `RUNBOOK.md` - from `templates/RUNBOOK.md`: where logs live, common failures,
  how to roll back, who to call.

## Gate 6 - Hand off to build

- Confirm a one-command verification loop exists (`make test`, `npm test`, `pytest`).
  If not, create it before any feature code.
- During build, use these when available: `test-driven-development` and
  `incremental-implementation` for every task; `code-simplification` before review;
  `doubt-driven-development` for high-stakes decisions (auth, regulated or personal data,
  money, irreversible migrations); any guardrail or testing skill the user has installed.
- Rule for the rest of the work: when implementation departs from `plan.md`, update
  `plan.md` in the same commit.

## Decisions after kickoff

Decisions keep changing after the spec is approved. Record each one in an append-only
`decisions.md` next to the spec (date, what changed, who decided, why, source link). If
`documentation-and-adrs` is available, use it for decisions that change architecture or a
public interface, and link the ADR from `decisions.md`.

## Retrofitting an existing prototype

If code already exists, run the gates in reverse order of risk: write `intent.md` from
what was observed and built, then `known-gaps.md`, then reconcile the existing spec and
tests against the intent. Say plainly in `intent.md` that it was written after the
prototype.

## Working with kspec

When kspec manages the repo, prefer its commands so its context and metrics stay correct:
- Gate 1: `kspec intent "<title>"` scaffolds `intent.md` and `known-gaps.md`; if that
  command is not available (older kspec), write them yourself in the active spec folder.
- Gates 2-3: `kspec spec` with no title builds the spec from `intent.md` in the same
  folder (older kspec: `kspec spec "<title>"` and paste `intent.md`), then `kspec design`
  and `kspec tasks`.
- After kickoff: `kspec drift` logs spec changes to `decisions.md` and the linked Jira
  issue; `kspec drift --check` gates CI.
This skill still owns Gate 4 (constraints) and Gate 5 (handover context).

## Companion skills

From `addyosmani/agent-skills`. Install all with `npx skills add addyosmani/agent-skills`,
or one with `--skill <name>`.

| Skill | Used in |
|-------|---------|
| `interview-me` | Gate 1 interview |
| `spec-driven-development` | Gate 2 requirements |
| `planning-and-task-breakdown` | Gate 3 tasks |
| `constraint-driven-development` | Gate 4 quality bar (`CONSTRAINTS.md`) |
| `code-review-and-quality` | Gate 5 review method |
| `test-driven-development`, `incremental-implementation`, `code-simplification`, `doubt-driven-development` | Gate 6 build |
| `documentation-and-adrs` | Decisions after kickoff |

If none are installed, mention once in the Gate 0 summary that installing them adds
review and quality-bar enforcement, then continue without them.

## Finish

Summarise the trail as a short list (file and one-line purpose), list the approvals still
open, and suggest the single next step. Do not start implementation until the user says so.
