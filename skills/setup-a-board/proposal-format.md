# The proposal format create_board takes

The same shape as a template (get_template shows a full example). Fields:

- **board:** `{id, name, request, questions: [{q, a}]}`. The id is lower-case letters, digits and `_`.
- **research:** one line per source read, with what it says. **sources:** `[{title, url}]`.
- **tables:** `[{name, label, plain, cols, stamps, allowed, files, states}]`
  - `label`: what people call the table, such as "Vital signs" for `vitals`. The board's screens show it.
  - `cols`: `{column: type}`. Types: text, int, numeric, boolean, date, timestamptz, jsonb.
  - `allowed`: the values a column may take, such as `{"visit_name": ["Screening", "Day 1", "Day 14", "Day 28"]}`. Give a list for every column that names a kind of thing (a visit, a status, a category). Anything else is refused, naming the list, and your rules must use exactly these values.
  - `stamps`: the columns access depends on, such as `site_code` or a yes/no column like `unblinded`.
  - `files`: columns that hold a file.
  - `states`: `[[from, to, role who may make the move]]`. The first state comes from `""`.
- **work:** `[{tbl, label, plain, title, tabs, done, card, cards}]`: the kinds of work, the tables whose rows are the records people work on. Each is listed on the left with how many are open, and each record opens as its own page. `cards` is the cards that page shows above the record's values, each `{card, heading?, sub?}` with `card` one of `facts`, `score-line`, `visits`, `latest`, `visit-form`, `files`, `questions`.
  - `tbl`: the table. `label`: what people call the records, such as "Visits". `plain`: one sentence on what one record is.
  - `title`: the columns that name one record, such as `["subject_no", "visit"]` for "104-012 Week 12".
  - `tabs`: the column whose values make the list's tabs, such as `status`; left out, the record's state. `done`: the values that mean finished; the rest count as open.
  - `card`: the card each record is shown with, if the board has one. A board that already exists names its work with `set_kinds_of_work`.
- **rules:** `[{id, tbl, kind, action, plain, expr, agent, within, source}]`
  - `kind`: `"value"` (refuses or asks), `"page"`, or `"must"` (an agent must happen, `within` a time such as `"5 days"`).
  - `expr`: one SQL expression over the row as `r.<column>` (and `r._state`, `r._by`, `r.id`), and the version before as `prev.<column>`. True means the rule holds.
  - A rule holds only when its answer is yes: an empty value makes a comparison unknown, and unknown counts as failing. Say `r.x is null or ...` where an empty value is fine. The answer's warnings list each value a rule fails on when it is empty.
  - To read other rows of this board, name the table: `exists (select 1 from <table> o where o.<column> = r.<column>)`; each row has its columns plus `id`, `_state` and `_by`. A row's past versions are in `history` (`id`, `ver`, `state`, `author`, `role`, `at`, `reason`). The tables can't be called `r`, `prev` or `history`.
- **access** (Who may): `[{id, tbl, tag, action, role, stamp, plain, cols}]`
  - `tag`: `"grant"` or `"never"`. `action`: `"read"` or `"write"`.
  - `role`: who it is for; `"*"` is anyone on the board. For a never, the role that is exempt.
  - `stamp`: only rows with this stamp: `<stamp column>:<value>`, such as `site_code:104`, or a yes/no stamp column alone, such as `unblinded`.
  - `cols`: for a write grant, only these columns may change.
- **roles:** `[{role, does, never, people, scope, scope_by_person}]`
  - `people`: account ids of people in the space. People who aren't in it yet are left to invite once the board exists.
  - `scope` limits a role to part of the data, in the same form as a stamp: `["site_code:101"]`. One role can serve every site: give each person their own scope with `scope_by_person: {"maria": ["site_code:101"], "tom": ["site_code:104"]}`.
- **agents:** `[{name, plain, started_by, steps: [{n, name, kind, owner, skill, next, ...}]}]`. A must-happen rule's `agent` is the agent's `name`. The kinds, with what each step also needs:
  - `"person"`: a role answers. `owner` (the role), `ask` (in plain words; `{key}` is the card's name), `within` (such as `"5 days"`), `from` (a date column on the row to count from; leave out to count from when the step starts), `ends_task` (true if the answer finishes the agent), `ask_in` (`"slack"` to also ask each of them in Slack, where a reply in the message's thread is their answer; only once the space has connected Slack), `options` (the answers they may give; left out, they answer in words), `send_to` (someone outside the board, such as a treating doctor: an email address, or the column of the row that holds one; they get one email with a link, sign in with a code sent to that address, and see only this step's form; `owner` may then be left out).
  - `"ai"`: the person's own AI does it. It has the same fields as a person step, plus `then_within` and `may_set` (the columns of the card it may suggest values for). Their AI lists it with `steps_waiting_for_my_ai` and finishes it with `hand_back_step`. Still waiting at its deadline, it becomes a person step.
  - `"send"`: a message: `to` (a role), `text`, an optional `subject`, and `connector` (`"email"` to email the people in that role who may see the card, `"slack"` to message each of them in Slack, or add `channel`, such as `"#mw201-sites"`, to post in that Slack channel instead; left out, it shows only in their Agents block).
  - `"wait"`: `for` (such as `"2 days"`) and an optional `from`.
  - `"check"`: one of the board's rules on the row as it is now: `rule` (its id) and `end_if` (`"holds"` ends the agent once the row is put right, `"fails"` ends it when it isn't).
  - `"extract"`: values pulled out of a text column: `from` (the column), `fields` ({column: one sentence on what it is}), `owner`, and optionally `sure` (0 to 1, default 0.9) and `then: {"if_unsure": "main_ai" | "person"}`.
  - `"decide"`: fixed questions with set options: `questions` ({column: {"ask": "...", "options": {"yes": "what it means", "no": "..."}}}), `owner`, optionally `in` (the columns it reads), `sure` and `then`.
  - `"main_ai"`: the main AI does `instructions` over `in` and fills `out` (a list of columns), with `owner` and optionally `sure`.
  - `"code"`: board code (`code`, its name, and an optional `input`). The code itself is saved after the board exists, with `save_board_code`. Until it is, the step stops its agent. **skills:** `[{name, plain, decide, extract}]`. **connectors**, **later**, **uncertain**: lists in plain words.
- **rows:** the rows the board starts with, such as the documents a reference model expects, partly filled: `[{tbl, key, state, set}]`. `key` names the row, the same way a sample does, so samples can act on a starting row (a coordinator filing an expected document). They are written with the board, by you, under its refusing rules, before the samples run. No file yet: people file them on the board. At most 300; bring the rest in later.
- **sample:** actions run on a throwaway copy of the board, as each person:
  - a write: `{as, role, tbl, key, state, set: {...}, reason, expect}`, where `expect` is `"ok"`, `"refused: <rule id>"` or `"asks: <rule id>"`;
  - a read: `{as, role, read: <table>, expect: "sees N of M"}`. Give one for each scoped role, showing it sees only its part.
  - A file column takes a file's name in a sample, such as `"cv.pdf"`: the throwaway copy stands in an uploaded file for it.
  - Every value rule needs one sample that breaks it and one that passes. The copy is rolled back: nothing a sample writes is kept, and the board is created with only its starting rows.

Limits for a first board: 5 tables, 12 rules, 3 agents, 6 roles, 3 skills, 4 kinds of work. A board has no pages of its own: don't send `pages`. Put the rest in `later`.
