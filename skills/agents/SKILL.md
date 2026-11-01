---
name: agents
description: Design a board's agents on MemBoard - a piece of work as the people doing it name its steps, each owned by a script, the AI or a person. Use when proposing agents, when work must happen every time, involves several people, or waits for days.
---

# Agents

An agent is a piece of work as the people doing it would name its steps. Each step has one owner: a short script, the AI, or a person.

## When something is an agent

- It must happen every time, involves other people, or waits for days. Otherwise it is free work in chat.
- Anything that must come back to someone becomes an agent, even a one-step one, so nothing waits inside a chat.
- A must-happen rule starts an agent: "a serious adverse event is reported within 7 days" starts "Report the event", due in 7 days.

## Steps follow the work, not the technology

- **A new step starts where work changes hands, waits, needs a decision, or leaves MemBoard.** Not at every database read or AI call: those happen inside a step.
- Name steps as the team does: "Check the source document", "Ask the site", "Sign off". Not "Fetch record" or "Call model".
- For each step, the team should be able to see its inputs, its outputs and its instructions.
- Each step names what comes next (`next`), which can depend on its outcome ("if the site answered, Close the query; if not, Chase the site").

## Who owns a step

| Owner | Use for | Note |
|---|---|---|
| `script` | Anything that is arithmetic or lookup: a date window, a total, a deadline | Prefer this. A board that runs on scripts, rules, forms and people needs no AI at all. The step kind is `code`; write the script with the board-code skill |
| `person` | Decisions, signatures, anything a regulation gives to a role | Owned by a role, not a named person. Someone outside the board (a treating doctor, a client's accountant) answers through `send_to`: an emailed link to that one step's form, and nothing else on the board |
| `ai` | Judgement: reading a messy document, drafting a reply | Uses one of the board's skills. Values it fills are suggested until a person confirms them |

Three kinds of step let MemBoard's own AI do the reading and sorting, each with a "how sure" line (`sure`, 0 to 1, default 0.9) and an `owner` (the role that checks what the AI isn't sure of):

- **extract**: pulls values out of a text column (`from`), such as a report a site sent; `fields` gives each column it fills with one sentence on what it is.
- **decide**: answers fixed questions with set options (`questions`: each column it answers, with `ask` and its `options`), such as "off-label: yes or no".
- **main_ai**: does the step's `instructions` over what it reads (`in`) and fills the columns in `out`, such as drafting a reply, when no fixed question fits.

What they fill lands on the record as suggested, with how sure the AI was. Below the line, `then.if_unsure` decides: `"main_ai"` tries the main AI on those values first, `"person"` (the default) asks the owner to check them. Prefer extract and decide to main_ai: they are cheaper and easier to measure.

Every run of these steps is kept. `agent_step_numbers` says how a step has done over its last runs: how often it was right against what people confirmed or changed, how many it sent up to a person, and the cost. Before changing such a step (its wording, questions or `sure` line), try the changed step with `try_agent_step` on the last runs and show the person the before and after. Change it only if they agree.

MemBoard's AI is paid for with the space's credits (`space_credits`); building and changing boards with you costs none. When a space runs out, its AI steps go to people, so a board never stops working for lack of credits.

Between those, three kinds of step need no owner's effort: a **send** (a message to a role, sent once), a **wait** ("give the site 2 more days"), and a **check**, which reads one of the board's rules on the row as it is now and can end the agent once the row is put right. The exact fields of each are in the setup-a-board skill's proposal-format.md.

On MemBoard today, a step owned by the AI waits for the person's own AI to pick it up. Their AI lists it with `steps_waiting_for_my_ai`, does it, and hands it back with `hand_back_step`. Values it hands back go on the step's card as suggested, only in the columns the step lists (`may_set`), and someone who may change them confirms them. A step still waiting at its deadline becomes a person step.

## The agent holds the logic

The order, the waits and the hand-offs live in the agent. Forms are the work. A "flow" drawn between forms is only a picture of the agent's steps.

## On a first board

At most 3 agents, each with its steps, owners and what comes next. Agents start when their rule says, within a minute of the save, and run on MemBoard with nobody's AI open. A deadline counts from when its step starts, or from a date on the row (`from`). An agent can also run on a schedule.
