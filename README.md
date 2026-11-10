# Meta Box skills for AI agents

An [agent skill](https://agentskills.io) that helps AI coding tools (Claude Code, Cursor, Codex, Copilot) write correct code for [Meta Box](https://metabox.io).

Without it, AI tools often mix Meta Box with ACF, invent extension names, or read field values from the wrong place. The skill lists the rules and points the agent to the [Meta Box docs](https://docs.metabox.io).

## Install

Claude Code:

```
/plugin marketplace add wpmetabox/meta-box-skills
/plugin install meta-box@meta-box
```

Other agents (Cursor, Codex, Copilot, and more):

```
npx skills add wpmetabox/meta-box-skills
```

Or copy `skills/meta-box/SKILL.md` to the skills folder of your agent, for example `.claude/skills/meta-box/SKILL.md`.

If your tool does not support skills, add the rules from [Coding with AI](https://docs.metabox.io/coding-with-ai/) to `AGENTS.md`, `CLAUDE.md`, or `.cursor/rules`.

## Content

The skill is a short summary of [Coding with AI](https://docs.metabox.io/coding-with-ai/). When you update that page, update `skills/meta-box/SKILL.md` too.

## License

GPL-2.0-or-later
