---
name: cards
description: Plan what people see on a MemBoard board - its kinds of work, the lists and record pages they open as, and the cards each record's page shows. Use when proposing a board's kinds of work or when a team asks to see something new.
---

# Kinds of work and cards

People never see rows. They see the board's work as records, and records as cards.

## The board's screens are fixed

A board has no pages of its own to design. Every board has the same screens:

- **Board home:** each kind of work with how many are open, and what waits for people.
- **A list for each kind of work:** a picture of how the work stands, tabs by state, and a card for each record.
- **A page for each record,** with its cards on top, then Details (each value with its state and who or which agent set it), Linked records and History.
- **How it runs:** Agents, Data, Connectors and Rules, for those who build the board.

So the design work is choosing the kinds of work well, and the cards their records show.

## Kinds of work

- A kind of work is a table whose rows are records people work on: patients, visits, inquiries, side effect cases. Name it as the team does ("Visits", not "Visit table").
- Say what names one record (`title`, such as `subject_no` and `visit`: "104-012 Week 12"), which column makes the tabs (`tabs`, such as `status`), and which of its values mean a record is finished (`done`).
- At most 4 kinds of work on a first board. A table that only supports others (vital signs under a visit) is not a kind of work; its rows show as linked records.

## Cards on a record's page

A card is about one thing in Data (a patient, a visit), a set, or a number. It can be a picture.

- A kind of work lists the cards its record's page shows (`cards`), each with an optional heading. A board can use the cards MemBoard has: `facts`, `score-line`, `visits`, `latest`, `visit-form`, `files` and `questions`.
- **A form is a card you may change.** The same visit card is a form for the coordinator entering it and a checklist for the monitor checking it. Who may decides which, never the card.
- Each value on a card shows its state: suggested (filled by an AI, dashed), confirmed, changed with a reason. Questions from asking rules sit on the card.
- **Reuse before making.** Use the cards the board already has (`describe_board`) before proposing a new one.

## Showing cards in chat

- `list_cards` shows a table's rows as cards, as the person may see them; `open_card` shows one with its history. Your AI app draws them as cards; say less, and let the card show it.
- `what_needs_me` shows what is waiting for the person across their boards.
- Confirming a value happens only on the card, when the person presses Confirm. Never say a value is confirmed because you sent it.

## Insights on Home

An insight is the answer to a question about the board, drawn the way that shows the finding best, on the board's Home: "open questions by site, per week", "which sites are slowest to sign". Offer one when the person asks a question they will want to see again. Only the board's builders may add one (`add_insight`).

1. **Write one SELECT over the board's tables by name.** Each table has its own columns and `id`, `_state`, `_by`, `_questions` (the ids of the rules with an open question on the row) and `_at` (when it last changed). Name every table with an alias and every column as `alias.column`. No WITH: use a subquery. Group so the answer is at most a few dozen rows.
2. **Return the records behind each row** as `array_agg(id) as _records`, so a click on that part opens them.
3. **Count one number from the records yourself** with `list_cards` or `open_card`, and give it as `check` (`column`, `total`, `how`). The insight is saved only if the query's total matches. If it doesn't, find out which is wrong; never change your count to match.
4. **Write the drawing** as the body of `draw(rows, el, open, theme)`: build an SVG (or HTML) picture of `rows` into `el`. Mark each clickable part with `data-row="<i>"`, or call `open(i)`. Use `theme`'s colours (`ink`, `soft`, `faint`, `line`, `teal`, `info`, `ok`, `warn`, `bad`, `violet`, `amber`) and one accent for the finding, grey for context. It runs sealed: it can't load anything or reach the network.
5. **Title it with the finding**, such as "Site 104 has most open questions this week", not the question.

Each person sees the insight drawn from only the rows they may see, so a site's coordinator sees only their site in it. `remove_insight` takes it off Home.

## What not to do

- Don't propose pages, page tiles or a published site: boards have none since 1 November.
- Don't design a list as a spreadsheet. A long table of rows is a list of cards with tabs by state ("To verify", "Verified").
- Don't use a screen to hide data. Who sees what comes only from Who may on the data.
