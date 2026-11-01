---
name: setup-a-board
description: Set up a new board when no template fits. Ask two or three questions, research how this work is normally done, and propose a small board for yes or no. Use when someone asks for a board for a kind of work and there is no template for it.
---

# Set up a board

A MemBoard skill for the person's own AI. The goal is a board that is **good enough to start**, about 60 to 70 percent of what the team will end up with. Not a complete system. The team finishes it by chatting.

## 1. Check for a template

If a template fits the request, offer it and stop here. Only carry on if none fits, or the team says no.

## 2. Ask at most three questions

Ask only what changes the board's shape. Skip any question the request already answers. Pick from:

1. **What are the kinds of work this board handles?** For example, "patients, visits and side effects", or "safety cases and the reports sent for them". Ask this first: each kind becomes a list on the left and its records' pages, and sets the main tables and agents. If the request already says, propose the kinds yourself and check them with the person.
2. **Who does the work, and who checks it?** These become the roles, and the separation-of-duty rules.
3. **Which rules or deadlines matter most, and under which regime?** For example, a country, a regulator or a standard. This sets the research.

Optional, never as a question: the team can drop in their SOPs, spreadsheets or forms. If they do, read them first. They outrank anything from the web.

## 3. Research

Use web search. Look for these six things, in this order:

| Look for | Why | Best sources |
|---|---|---|
| The steps of the work, as practitioners name them | The agent's domain steps | Regulator guidance, standards bodies, professional bodies, then practitioner guides |
| The records and documents involved, with their key fields | The tables and their columns | Official forms and data standards |
| Deadlines and clocks | Must-happen rules with a "within" | The regulation itself |
| Checks that must always hold | Value rules (refuse or ask) | The regulation, then official guidance |
| Who may and may not do what | Roles, Who may, Never | The regulation, then common practice |
| Judgement calls in the work | Skills, with the questions to put to `decide` and the fields for `extract` | Guidance and practitioner material |

Rules for research:

- **Read the source before using it.** Cite only pages actually read, with links.
- **Prefer primary sources:** the law, the regulator, the standard. Use vendor blogs only for what practitioners call things.
- **Check the date.** Rules change. Say which version or year a rule comes from, and flag anything recently changed or under review.
- **Stop after about 6 to 10 sources.** Enough to be right about the common case, not complete.
- **No team data in queries.** Search with the team's answers about their kind of work, never with records from a board.

## 4. Propose, on one page

Keep it small, and stay inside these limits. A team can always ask for more.

| Part | At most | Each item says |
|---|---|---|
| Kinds of work | 4 | Which table holds the records, what people call them, what names one record, its tabs and which values mean done |
| Tables | 5 | Columns, states, which columns hold files |
| Rules | 12 | Plain words, refuse or ask, the source |
| Agents | 3 | Domain steps (script, AI or person), who owns each, what comes next |
| Roles | 6 | What they do, and what they may never do |
| Skills | 3 | What judgement they hold, the questions for `decide`, the fields for `extract` |
| Connectors | 3 | Named only, not built in setup |

- **For each part, follow its skill** (read it with read_skill): tables-and-allowed-values, rules, cards, agents, connectors, board-code.
- **Lean on rules, forms, people and connectors.** A step that can be computed is a script; the AI is for judgement only. Most boards should run with no AI at all.
- **Every item has one line on why, and a source** (a citation, or "your SOP", or "common practice", saying which).
- **Every item is yes or no for the team.**
- **Not exotic.** Only what the research shows is common. Anything rarer, or needing a connector or data the team hasn't given, goes on a **"Later"** list with one line each.
- **Flag uncertainty.** If sources disagree, or a rule depends on the team's situation (size, country, type of client), say so, and pick the common case.

## 5. Test, then create

- Write the proposal in the format of `proposal-format.md` (read it with read_skill), and test it with create_board, mode `"test"`. Fix it until every sample does what it expects and every value rule has refused or asked at least once.
- Show the team the one-page proposal in plain words. Create the board (mode `"create"`) only after they say yes.
- The samples run on a throwaway copy that is rolled back. **The board is created empty**: never tell the team it has sample records.
- Roles name people by their account ids, from list_boards (each space lists its people). People named in roles who aren't in the space yet are listed to invite; say so.
- Afterwards, work on the board as the use-this-board-well skill says.
