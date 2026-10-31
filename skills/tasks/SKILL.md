---
name: tasks
description: Design a board's tasks on MemBoard - a piece of work as the people doing it name its steps, each owned by a script, the AI or a person. Use when proposing tasks, when work must happen every time, involves several people, or waits for days.
---

# Tasks

A task is a piece of work as the people doing it would name its steps. Each step has one owner: a short script, the AI, or a person.

## When something is a task

- It must happen every time, involves other people, or waits for days. Otherwise it is free work in chat.
- Anything that must come back to someone becomes a task, even a one-step one, so nothing waits inside a chat.
- A must-happen rule starts a task: "a serious adverse event is reported within 7 days" starts "Report the event", due in 7 days.

## Steps follow the work, not the technology

- **A new step starts where work changes hands, waits, needs a decision, or leaves MemBoard.** Not at every database read or AI call: those happen inside a step.
- Name steps as the team does: "Check the source document", "Ask the site", "Sign off". Not "Fetch record" or "Call model".
- For each step, the team should be able to see its inputs, its outputs and its instructions.
- Each step names what comes next (`next`), which can depend on its outcome ("if the site answered, Close the query; if not, Chase the site").

## Who owns a step

| Owner | Use for | Note |
|---|---|---|
| `script` | Anything that is arithmetic or lookup: a date window, a total, a deadline | Prefer this. A board that runs on scripts, rules, forms and people needs no AI at all. The step kind is `code`; write the script with the board-code skill |
| `person` | Decisions, signatures, anything a regulation gives to a role | Owned by a role, not a named person |
| `ai` | Judgement: reading a messy document, drafting a reply | Uses one of the board's skills. Values it fills are suggested until a person confirms them |

Between those, three kinds of step need no owner's effort: a **send** (a message to a role, sent once), a **wait** ("give the site 2 more days"), and a **check**, which reads one of the board's rules on the row as it is now and can end the task once the row is put right. The exact fields of each are in the setup-a-board skill's proposal-format.md.

On MemBoard today, a step owned by the AI waits for the person's own AI to pick it up. Their AI lists it with `steps_waiting_for_my_ai`, does it, and hands it back with `hand_back_step`. Values it hands back go on the step's card as suggested, only in the columns the step lists (`may_set`), and someone who may change them confirms them. A step still waiting at its deadline becomes a person step.

## The task holds the logic

The order, the waits and the hand-offs live in the task. Forms are the work. A "flow" drawn between forms is only a picture of the task's steps.

## On a first board

At most 3 tasks, each with its steps, owners and what comes next. Tasks start when their rule says, within a minute of the save, and run on MemBoard with nobody's AI open. A deadline counts from when its step starts, or from a date on the row (`from`). A task can also run on a schedule.
