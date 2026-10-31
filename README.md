# MemBoard for Claude

Adds MemBoard to Claude: the MemBoard connector, which lets Claude work on your boards as you, and the skills that teach Claude to build a board well. MemBoard lets a team build the software it works in without programming: a board is a stack of pages over its own data, rules, tasks and skills, built and changed by talking to your AI, and kept with every version of every value.

| Part | What it is |
|---|---|
| [.claude-plugin/plugin.json](.claude-plugin/plugin.json) | The plugin's manifest |
| [.mcp.json](.mcp.json) | The MemBoard connector at `https://memboard.ai/mcp`. Claude signs you in with your email and a one-time code the first time |
| [skills/](skills) | setup-a-board, tables-and-allowed-values, rules, pages-and-cards, tasks, connectors, board-code, use-this-board-well |

## What it runs, connects to and sends

The plugin runs nothing on your computer: no hooks, scripts or local servers. It holds skills (text that teaches Claude how to build a board) and one remote MCP server, `https://memboard.ai/mcp`. Claude talks to that server only when it calls a MemBoard tool, sending the tool's inputs, as you, after you have signed in to MemBoard and allowed Claude. MemBoard never receives your conversations with Claude.

How to use it, and every tool it offers: [memboard.ai/help](https://memboard.ai/help). How MemBoard handles your data: [Privacy policy](https://memboard.ai/privacy). Help: support@memboard.ai.

## Install

- **Claude Code:** add MemBoard from the Claude plugin directory, or from a copy of this folder: `claude --plugin-dir <this folder>`.
- **Claude on the web, desktop and mobile:** add MemBoard from the connectors directory, or add `https://memboard.ai/mcp` as a custom connector. The connector serves the same skills, so you get them there too.
