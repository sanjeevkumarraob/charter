---
name: kickoff
description: Use before starting any new app, feature, prototype or bridge tool, at work or on a personal project, to set up an AI-native SDLC trail before code is written - intent, spec, plan, known gaps, agent context (CLAUDE.md, AGENTS.md), review rules and a runbook. Keywords - kickoff, intent.md, spec-driven, plan mode, handover, known gaps, vibe coding, AI SDLC.
---

# Kickoff

Goal: before any code is written, produce a committed artifact trail that makes AI-built
work traceable, reviewable and supportable by someone who was not in the room. The trail
answers "it's vibe coded" with evidence: who asked for what, why, what was decided, what
was built, and what is still deliberately unfinished.

Work through the gates in order. Each gate ends with a file and a pause for human
approval. Never skip a gate silently; if the user skips one, record it in `known-gaps.md`.
Templates for every file are in `templates/` next to this SKILL.md.

## Gate 0 - Detect context

1. Look for existing structure: `CLAUDE.md`, `AGENTS.md`, `.kiro/`, `specs/`, `docs/`,
   ADR folders, `.github/`. Reuse what exists; never add a parallel structure.
2. Choose the artifact home:
   - `.kiro/` present and kspec installed -> write `intent.md` and `known-gaps.md` into the
     active kspec spec folder and let kspec own spec/design/tasks (see "Working with kspec").
   - Otherwise -> `docs/sdlc/<kebab-slug>/`.
3. Ask one question: is this work (shared ownership, regulated data, other teams involved)
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

Write `intent.md` from `templates/intent.md`. Ask the user to have the business owner
review it. Stop for approval.

## Gate 2 - Spec (`spec.md`)

From the approved intent, write requirements and design using `templates/spec.md`. Apply
any security, compliance or UX skills available in the session. Requirements must be
testable; use "When <trigger>, the system shall <response>" where it helps. Always fill
the Concerns section: policy conflicts, overlap with another team's roadmap, data risks.
Stop for approval.

## Gate 3 - Plan (`plan.md`)

Plan without editing files (use plan mode or the agent's read-only mode if it has one).
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

## Gate 5 - Agent and review context

Create or update, each under about a page:
- `CLAUDE.md` - commands with healthy output, conventions, architecture, "things the agent
  gets wrong", and a Verifying section (run build, tests, lint before reporting done;
  fix code, not tests).
- `AGENTS.md` - the same essentials, tool-neutral, for Kiro, Copilot, Cursor, Codex and
  others. If the repo prefers one source, make one file point at the other.
- `REVIEW.md` - from `templates/REVIEW.md`: review passes, what counts as Important,
  and a rule to flag diff content not traceable to `plan.md`.
- `RUNBOOK.md` - from `templates/RUNBOOK.md`: where logs live, common failures,
  how to roll back, who to call.

## Gate 6 - Hand off to build

- Confirm a one-command verification loop exists (`make test`, `npm test`, `pytest`).
  If not, create it before any feature code.
- If a guardrail or testing skill is available, apply it during build.
- Rule for the rest of the work: when implementation departs from `plan.md`, update
  `plan.md` in the same commit.

## Retrofitting an existing prototype

If code already exists, run the gates in reverse order of risk: write `intent.md` from
what was observed and built, then `known-gaps.md`, then reconcile the existing spec and
tests against the intent. Say plainly in `intent.md` that it was written after the
prototype.

## Working with kspec

When kspec manages the repo, this skill owns Gate 1 (intent), Gate 4 (known gaps) and
Gate 5 (handover context). Hand Gates 2 and 3 to kspec: after intent is approved, tell the
user to run `kspec spec "<title>"` (or `/kspec-spec`) and paste `intent.md` as the input,
then `kspec design` and `kspec tasks`.

## Finish

Summarise the trail as a short list (file and one-line purpose), list the approvals still
open, and suggest the single next step. Do not start implementation until the user says so.
