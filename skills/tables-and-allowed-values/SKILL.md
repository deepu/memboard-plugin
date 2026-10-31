---
name: tables-and-allowed-values
description: Design a board's tables (its Data) with MemBoard - columns, types, allowed values, states, stamps for who sees what, and file columns. Use when proposing a board or adding to its Data.
---

# Tables and allowed values

A board's Data is its tables. It is the only place with rows; pages show them as cards. Get the tables right and everything else is easy.

## One table per kind of thing

- A table is one kind of thing the work is about, as the team names it: a patient, a visit, a query, a return, a document.
- **Something expected is a row before it exists.** If the work expects four visits or twelve documents, those are rows with a state like `expected`, partly filled, not a checklist somewhere else.
- At most 5 tables on a first board. A join table or a lookup list is rarely needed: use `allowed` values instead.
- Name tables and columns in lower case with `_`: `visit`, `visit_on`, `systolic_bp`.

## Columns and types

Types: `text`, `int`, `numeric`, `boolean`, `date`, `timestamptz`, `jsonb`.

- A date the team writes on paper is a `date`. Use `timestamptz` only for a moment, such as when a sample was taken.
- Money and measurements are `numeric`; counts are `int`.
- Keep the unit in the column name: `weight_kg`, `amount_usd`.
- Avoid `jsonb` unless the shape truly varies.

## Allowed values: always, for anything that names a kind of thing

Give an `allowed` list for every column that names a kind of thing: a visit, a category, a severity, a site, a document type.

```json
"allowed": {"visit_name": ["Screening", "Day 1", "Day 14", "Day 28"], "severity": ["mild", "moderate", "severe"]}
```

- Anything else is refused, and the refusal names the list, so a person (or you) can correct it at once.
- **Your rules must use exactly these values.** A rule that compares a column with a value outside its list is refused before the board is made (`'day14'` when the list says `Day 14`).
- Lists are for text and number columns. A yes/no column is already a list.

## States

A table can have states, such as `entered` → `verified` → `signed_off`, written as moves: `[from, to, role]`. The first move comes from `""` (a new row).

- Each move says which role may make it. That is how "a coordinator never verifies their own data" is kept: only the monitor's role may move `entered` to `verified`.
- A move back (reopen) is its own line, with its own role.
- States are what pages tab on ("To verify", "Signed off"), so name them as the team says them.

## Stamps: who sees which rows

`stamps` are the columns that access depends on. Every row is stamped with their values, and Who may and roles' scopes use them.

- A value stamp: `site_code`. A role is scoped as `site_code:101`; a Who may line as `"stamp": "site_code:104"`.
- A yes/no stamp: `unblinded`. Named alone.
- Put the stamp column on every table whose rows belong to someone (a site, a client, a team). A visit carries its patient's `site_code` too.

## Files

List columns that hold a file in `files`. The file itself goes into the board's document store; the column holds which one. A document the work keeps (a signed form, a CV, a certificate) is a file column on its row, not a link to somewhere else: then Who may, versions and signing apply to the file too.

## Rows the work expects

When the work is a known list to fill (the documents a reference model expects, the checks of an inspection, the steps of a course), give the board those rows from the start, partly filled: what each is, whose it is, and a first state such as `expected`. They go in the proposal's `rows`. Then "what is missing" is simply the rows still in their first state.

## Check before proposing

- Every column a rule reads exists on its table.
- Every kind-of-thing column has an `allowed` list.
- Every table someone should see has a read grant (see the rules skill for Who may).
