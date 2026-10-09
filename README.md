# ai-native-sdlc-skills

Agent skills for running an AI-native software lifecycle: capture intent before code,
name the gaps honestly, and leave a trail any engineer (or coding assistant) can support.

Works with any agent that reads Agent Skills: Claude Code, Kiro, GitHub Copilot, Cursor,
Codex and others.

## Install

```bash
# all skills, for every agent on this machine
npx skills add sanjeevkumarraob/ai-native-sdlc-skills -g

# just one skill, into the current project
npx skills add sanjeevkumarraob/ai-native-sdlc-skills --skill kickoff
```

## Skills

| Skill | Use it when | Produces |
|-------|-------------|----------|
| `kickoff` | Before any new app, feature, prototype or bridge tool | `intent.md`, `spec.md`, `plan.md`, `known-gaps.md`, `CLAUDE.md`, `AGENTS.md`, `REVIEW.md`, `RUNBOOK.md` |

Then run `/kickoff <one line about the work>`, or just ask your agent to "kick off" a new project.

## Works with Addy Osmani's agent-skills

`kickoff` covers what comes before and around the code: intent, ownership, sunset,
known gaps, handover and decisions. For build quality it hands off to
[addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) when installed
(`interview-me`, `spec-driven-development`, `constraint-driven-development`,
`code-review-and-quality` and others), and falls back to its own steps when not.

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
