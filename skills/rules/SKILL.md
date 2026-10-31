---
name: rules
description: Write a board's rules with MemBoard - checks on a value (refuse or ask), rules on a card, must-happen deadlines, and Who may (grant or never). Use when proposing a board, adding a rule, or when a team says something must always or never happen.
---

# Rules

A rule is a sentence in plain words plus one short SQL expression that checks it. MemBoard runs it the same way every time, on every change, whoever makes it: a person, a task or you. When a rule refuses, the person sees its plain words.

## The four kinds

| Kind | What it does | Example |
|---|---|---|
| On a value (`value`) | Checks a row whenever it changes. `refuse` stops the change; `ask` lets it through and puts a question on the card | "Systolic blood pressure is between 70 and 220" |
| On a card (`page`) | Says which parts of a card show for this row | "Show the unblinding section only to unblinded staff" |
| Must happen (`must`) | When its expression holds, a task starts, due `within` a time | "A serious adverse event is reported within 7 days" |
| Who may (`access`) | Grants read or write on a table, or says never | "Coordinators see only their own site's visits" |

## Writing the expression

- One SQL expression over the row as `r.<column>`, plus `r._state` (its state), `r._by` (who is changing it) and `r.id`. The version before is `prev.<column>`.
- **True means the rule holds.**
- **A rule holds only when its answer is yes.** An empty value makes a comparison unknown, and unknown counts as failing. Decide on purpose: `r.systolic is null or r.systolic between 70 and 220` lets an empty value through; `r.visit_on <= current_date` alone fails when it is empty. On a new row `prev` is empty, so start a rule about changes with `prev.id is null or`. The answer to `propose_rule` lists, under `empty_values`, the values the rule fails on when they are empty.
- Rules about a state: `r._state is distinct from 'signed_off' or r.signoff_meaning is not null`.
- To read other rows of the board, name its table, with the same columns plus `id`, `_state` and `_by`: `not exists (select 1 from query q where q.visit_ref = r.id and q._state = 'open')`. A row's past versions are in `history` (`id`, `ver`, `state`, `author`, `role`, `at`, `reason`): `not exists (select 1 from history h where h.id = r.id and h.state = 'entered' and h.author = r._by)`.
- Every rule is read by Postgres's own SQL parser before it is saved. It may use only `r`, `prev`, the board's tables and `history`, and plain SQL: comparisons, dates, `case`, `coalesce`, `exists`, and common functions such as `lower`, `round`, `abs`, `extract`, `age`, `make_date`, `count`. Anything else is refused with the reason, such as another board's table or a function that reads files. Every column it names must exist.
- Use the exact `allowed` values from the table. A value outside the list is refused before the board is made.

## Refuse or ask

- **Refuse** what can never be right: an impossible value, a date in the future, a signature without its meaning.
- **Ask** what is usually wrong but sometimes right: a visit outside its window, a value far from the last one. The change goes through and a question sits on the card until someone fixes it.
- When unsure, ask. A refused change is lost work; a question costs a moment.

## Who may

- `grant` read or write on a table to a role, optionally only on rows with a stamp (`site_code:104`), and for writes only on some columns (`cols`).
- `never` beats any grant. Its `role` is who is exempt.
- Putting a card on a page never grants anything. Only these lines do.
- Separation of duty is a state move plus Who may: the person who entered a row's role cannot make the move that checks it.
- A role's scope (`site_code:101`) limits a person to their part of the data. One role can serve every site with `scope_by_person`.

## Testing

- Every value rule needs one sample that breaks it and one that passes. create_board refuses a board where a value rule never refused or asked.
- A new rule on a live board goes through `propose_rule`: it is tested on every past version of the table first. Tell the person how many past versions would fail, and go live only after they say yes.

## Plain words

The `plain` sentence is what people read when a change is refused. Write it as the team would say it, without SQL or jargon, and give its `source`: a regulation, "your SOP", or "common practice".
