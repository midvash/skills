# Midvash skills

Agent skills for studying the Bible, by [Midvash](https://midvash.com). They work
in Claude Code, Claude.ai, Cursor, Codex and any agent that reads `SKILL.md`.

```bash
npx skills add midvash/skills
```

## Skills

| Skill | What it does |
|---|---|
| [`bible-study`](skills/bible-study/SKILL.md) | Guides a real Bible study (observation, interpretation, application) on a passage or a topic, as a personal study, a small-group lesson or a sermon outline. Every verse is fetched, never quoted from memory, and linked to the full chapter on midvash.com. |

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
