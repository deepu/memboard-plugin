---
name: board-code
description: Write small scripts for a board on MemBoard - code sealed to its board that works out values for a task step. Use when a step is arithmetic or lookup (a date window, a total), never for reaching the internet.
---

# Board code

Board code is a small script you write for a board, such as a task step that works out a visit window or totals a return. It runs on MemBoard, sealed to its board.

## What it can and can't do

- **It can read and write only its own board,** acting as the person or task it runs for, through the same rules and Who may as everyone else. Everything it writes is in History, naming the code and its version.
- **It can't reach anything else:** no internet, no other board, no files. Anything outside goes through the board's connectors.
- **It is stopped** if it runs too long, reads too much or calls too often, and History says why.
- **It is tested before it goes live,** like any change to the board.

## When to use it

- A step that is arithmetic or lookup: a date window, a deadline that skips weekends, a total, a score from a table.
- Never for judgement (that is a skill and an AI step) and never for something a rule says better. A check that must always hold is a rule, not code.

## How to write it

- One script does one job and calls no other script.
- Plain names, a comment saying what it works out and from what.
- Prefer a script to an AI step whenever the answer can be computed: it is faster, free, and gives the same answer every time.

## Writing it

- Save it with `save_board_code`: a name (such as `visit-window`), one plain sentence on what it works out, the source, and at least one test.
- The source is one JavaScript module: `export default async (input, gate) => result`. `input` holds what the task step gives it: the run's card (`input.row`, `input.key`) and the step's own `input`.
- `gate` is the only way to the board. `await gate.read('visit')` gives the cards of a table you may see, `await gate.card(id)` gives one card, and `await gate.write(table, id, {column: value}, {reason})` changes one. Each card is `{id, ver, state, values, questions}`.
- Return plain JSON, such as `{from: '2026-11-04', to: '2026-11-18'}`. A later step's words can name each field: "Book {key} between {from} and {to}."
- A test is `{name, input, expect}`. The code runs sealed against the board as it is now, and `expect` must match exactly. In a test, writes are only noted.
- If every test passes, the new version is live. If one fails, the version stays out of use, and you are told why. Fix it and save again.
- In a task, the step is `{"kind": "code", "code": "visit-window", "input": {...}}`.
