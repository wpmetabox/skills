# Meta Box skills

An [agent skill](https://agentskills.io) for [Meta Box](https://metabox.io). It tells AI tools how to write correct Meta Box code, lists the mistakes that AI tools make most often, and links to the [Meta Box docs](https://docs.metabox.io).

## Install

Claude Code:

```
/plugin marketplace add wpmetabox/skills
/plugin install meta-box@meta-box
```

Cursor, Codex, Copilot, and other agents:

```
npx skills add wpmetabox/skills
```

You can also copy `skills/meta-box/SKILL.md` to the skills folder of your agent, for example `.claude/skills/meta-box/SKILL.md`.

## Update

The skill is a short version of [Coding with AI](https://docs.metabox.io/coding-with-ai/). When you change that page, change `skills/meta-box/SKILL.md` too.

## License

GPL-2.0-or-later
