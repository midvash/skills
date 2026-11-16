# Midvash skills

Agent skills for studying the Bible, by [Midvash](https://midvash.com). They work
in Claude Code, Claude.ai, Cursor, Codex and any agent that reads `SKILL.md`.

```bash
npx skills add midvash/skills
```

One skill only: `npx skills add midvash/skills --skill sermon-prep`.

## Skills

| Skill | What it does |
|---|---|
| [`bible-study`](skills/bible-study/SKILL.md) | Guides a real Bible study (observation, interpretation, application) on a passage or a topic, as a personal study or a small-group lesson. Every verse is fetched, never quoted from memory, and linked to the full chapter on midvash.com. |
| [`sermon-prep`](skills/sermon-prep/SKILL.md) | Prepares a sermon from the text up: studies the passage, states its big idea, builds the points the text itself gives and times each block for the length you preach. Also plans a series through a book. |

## Where the text comes from

The skills read Scripture from the free [Midvash Bible MCP](https://mcp.midvash.com)
(119 public-domain and openly licensed translations in 69 languages, no key).
When the MCP server is not connected, they fall back to the public REST API at
[api.midvash.com](https://api.midvash.com).

**Claude Code plugin:** installing this repository as a plugin also connects the
MCP server (see [`.mcp.json`](.mcp.json)).

```bash
/plugin marketplace add midvash/skills
/plugin install midvash-skills@midvash
```

Elsewhere, add `https://mcp.midvash.com/mcp/plugin` as an MCP server, or create
a personal link with your preferred versions at [mcp.midvash.com](https://mcp.midvash.com).

## License

MIT
