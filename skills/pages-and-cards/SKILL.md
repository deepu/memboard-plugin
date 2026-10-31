---
name: pages-and-cards
description: Plan a board's pages and cards on MemBoard - pages as documents that use cards, which cards to reuse, and how a card opens as a page. Use when proposing a board's pages or when a team asks for a new view.
---

# Pages and cards

People never see rows. They see pages, and pages show cards.

## A page is a document

A page has a header, headings, short prose and notes, with cards placed among them, like a well-written memo that happens to be live. It can be a form to fill in, a collection of cards, a picture, or a page to sign.

- Name a page for the work it serves, as the team would: "Visits to clear", "Queries due this week", "Road to lock". Not "Visit table".
- Open with one picture or number that says how things stand, then the cards that need doing, then the rest.
- At most 5 pages on a first board. Each says in one line (`plain`) what it shows and for whom.

## A card is one well-defined thing

A card is about one thing in Data (a patient, a visit), a set (open queries at Leeds), or a number (212 items to lock). It can be a picture.

- **A form is a card you may change.** The same visit card is a form for the coordinator entering it and a checklist for the monitor checking it. Who may decides which, never the page.
- **Every card opens as a page of its own,** with the same data, wherever it is clicked from.
- Each value on a card shows its state: suggested (filled by an AI, dashed), confirmed, changed with a reason. Questions from asking rules sit on the card.
- **Reuse before making.** Use the cards the board already has (`describe_board`) before proposing a new one.

## Showing cards in chat

- `list_cards` shows a table's rows as cards, as the person may see them; `open_card` shows one with its history. Your AI app draws them as cards; say less, and let the card show it.
- `what_needs_me` shows what is waiting for the person across their boards.
- Confirming a value happens only on the card, when the person presses Confirm. Never say a value is confirmed because you sent it.

## What not to do

- Don't design pages as spreadsheets. A long table of rows is a collection of cards with tabs by state ("To verify", "Verified").
- Don't use a page to hide data. Who sees what comes only from Who may on the data.
