# charter

Agent skills for running an AI-native software lifecycle: capture intent before code,
name the gaps honestly, and leave a trail any engineer (or coding assistant) can support.

Works with any agent that reads Agent Skills: Claude Code, Kiro, GitHub Copilot, Cursor,
Codex and others.

## Install

```bash
# all skills, for every agent on this machine
npx skills add sanjeevkumarraob/charter -g

# just one skill, into the current project
npx skills add sanjeevkumarraob/charter --skill kickoff
```

## Skills

| Skill | Use it when | Produces |
|-------|-------------|----------|
| `kickoff` | Starting something new: an app, service, prototype or bridge tool | `intent.md`, `spec.md`, `plan.md`, `known-gaps.md`, `CLAUDE.md`, `AGENTS.md`, `REVIEW.md`, `RUNBOOK.md` |
| `change` | Changing an existing codebase: feature, fix, refactor, migration | `codebase-map.md` (grows per change), `change.md`, characterization tests, PR description, area specs |

Run `/kickoff <one line about the new thing>` or `/change <ticket or one line about the change>`.

### Which one?

- **No code yet** → `kickoff`. Intent, ownership and sunset come first.
- **Code exists** → `change`. It sizes each change by risk first, so a typo fix gets no
  paperwork and a schema change gets all of it. It grows a codebase map one change at a
  time, naming exemplar files so agents copy how the codebase already works instead of
  inventing new ways. It pins current behaviour with tests before touching anything, and
  writes a PR description reviewers can check the diff against. Steps that change no
  behaviour run unattended; anything that does waits for approval.

## Works with Addy Osmani's agent-skills

`kickoff` and `change` cover what comes before and around the code: intent, ownership,
blast radius, known gaps, handover and decisions. For build quality they hand off to
[addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) when installed
(`interview-me`, `spec-driven-development`, `constraint-driven-development`,
`code-review-and-quality`, `debugging-and-error-recovery`, `deprecation-and-migration`
and others), and fall back to their own steps when not.

```bash
npx skills add addyosmani/agent-skills -g
```

## Why

Spec-driven tools (Spec Kit, Kiro, kspec) start at the spec. Teams usually push back on
AI-built work for reasons that sit *before* the spec: who owns it, who supports it at
2am, does it clash with another team's roadmap, when does it retire, and what is still
prototype-grade. This skill makes those answers the first artifact.

Using kspec with Kiro? The skill detects `.kiro/` and hands spec, design and tasks to
kspec, keeping intent, known gaps and handover context for itself.

## Credits

Process follows Anthropic's
[AI-native SDLC playbook](https://claude.com/resources/articles/the-ai-native-sdlc-playbook).

## License

MIT
