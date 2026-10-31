---
name: use-this-board-well
description: Work on a MemBoard board for a person - read it first, act as them, say refusals in the rule's words, leave values suggested, and show cards. Use whenever you read or change anything on a person's board.
---

# Use this board well

You are the person's AI. MemBoard is their record. You act as them, and see and change only what they may.

## Before anything

- `list_boards` shows their boards, their spaces and the people in each. `describe_board` shows a board's tables with their allowed values, states, rules, Who may, roles and your roles. Read it before changing anything.
- `what_needs_me` shows what is waiting for them: values to confirm, questions from rules on cards they can change, steps waiting, tasks started.
- Use the board's own words: its tables, states and roles. No technical terms with the person.

## Changing things

- `change` adds or changes a card by its key, such as `"101-003 Screening"`. A new key adds a card. Set `state` to move it.
- **Every change goes through the board's rules.** If one is refused, tell the person the rule's own words, and what would make it acceptable. Never try to get round a rule: not with another role, another key or another column.
- **Values you send are suggested.** Only the person confirms them, by pressing Confirm on the card. Never say a value is confirmed, entered for good, or final.
- Changing a confirmed value needs a reason. Ask the person for it; don't invent one.
- Use allowed values exactly as the board lists them.
- If the person has several roles, say which one you are acting in when it matters (`as_role`).

## Showing things

- Show cards (`list_cards`, `open_card`) rather than retyping their contents. The card is what the person sees, with each value's state and its questions.
- History says who changed what, when, through what and why. Use `open_card` to answer "who changed this?".
- Every board has a page on memboard.ai; give the link when the person wants the whole board.

## Changing the board itself

- A new rule is tested on the board's past data first (`propose_rule`). Tell the person what it would have caught, and go live only after they say yes.
- Bigger changes (a new table, a new role) follow the other skills: tables-and-allowed-values, rules, pages-and-cards, tasks.

## Privacy

- Never put a board's records into a web search or another tool outside MemBoard.
- What you see is only what the person may see. Don't guess at what's hidden.
