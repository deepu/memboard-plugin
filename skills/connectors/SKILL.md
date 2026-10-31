---
name: connectors
description: Plan a board's connectors on MemBoard - other systems (an EDC, a lab, email, Slack) mapped onto the board's tables. Use when a team mentions another system their data lives in or that people must be reached through.
---

# Connectors

A connector is another system, such as Veeva, a lab, email or Slack. It is mapped onto the board's own tables, and gives tasks and chat tools to use it.

## How a connector fits

- **It fills the board's tables, it doesn't replace them.** Pages, rules and Who may work the same whether a row came from a person or a connector.
- **You propose the mapping:** which of the system's fields go to which columns, and how often it refreshes. The team says yes or no.
- **When both sides change a value, the outside system wins.**
- **Syncs run on a space account, not one person's login.** For a connector that is an MCP server, the board gives a memboard.ai link where someone signs in, and the board keeps that sign-in, so tasks can use it while nobody's AI is open. The sign-in is never shown to board code or to any AI.
- **People outside MemBoard are reached through connectors** too: email, Slack, Teams. A notification is a Home item sent out.

## On MemBoard today

- **In a proposal**, name each system in `connectors` with one line on what it would fill or send. Design the tables so that rows can be entered by hand until it is connected.
- **Email and Slack** are a task step's `connector` (see the proposal format).
- **An outside MCP server** can fill a table once the board exists:
  1. `add_connector` with the server's address. Give the person the memboard.ai link it returns; they sign in to the server there. Never ask for a password or key in chat.
  2. `connector_tools` lists its tools; with `tool`, it calls one for a sample.
  3. `map_connector` with `try: true`: which field fills which column, the outside record's own id as `key`, a role that may write the table as `as_role`, and how often (`every`). Show the person the preview rows in plain words.
  4. Save with the same call, without `try`, only after they say yes. `list_connectors` shows how each pull went.
- **A connector's data that breaks a rule comes in with a question on it**, even under a refusing rule, because the outside system already holds it. Tell the team who will see those questions: the people in the roles that may change the card.
- Other systems (Veeva, a lab's files) aren't built yet: put what depends on them in `later`.

## When proposing one

- Say which tables and columns it fills, and which it reads to send.
- Say how fresh the data needs to be (every few minutes, hourly, nightly), and why.
- Say what happens if it is down: work carries on by hand, or waits.
