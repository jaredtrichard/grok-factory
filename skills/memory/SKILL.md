---
name: Coverage memory
description: Use whenever a name researcher reads or writes durable facts about a name under coverage.
---

# Coverage memory

Memory is the mind. Sqlite routes. Chat is neither.

Each name has one home tree on the shared Grok Bot computer:

`/home/box/agent-data/grok-factory/memory/<name id>/`

Create the directory when that name is taken under coverage. Do not create it for a scan candidate that was never taken.

## Read first

Open `INDEX.md` before any other memory file. It says which home owns which kind of fact. Then open only the home you need.

If `INDEX.md` is missing, write it, then continue.

## Homes

| file | owns |
| --- | --- |
| `INDEX.md` | map of homes; no facts |
| `register.md` | facts, KPIs, citations, `not obtained` |
| `thesis.md` | working thesis and killing conditions |
| `model.md` | pointer to `models/<name id>/` and reconciliation notes |
| `consensus.md` | what is already priced; views around the name |

One home per fact. If a number lives in the workbook, `model.md` points at the line. Do not copy that number into `register.md` or `thesis.md`.

`thesis.md` is the working mind. The staged file `theses/<task id>.md` is what Firstmate takes to the captain. `names.thesis_ref` is the published pointer. Do not treat those three as interchangeable.

## INDEX.md

Keep it short:

```
# <name> memory

Read this first. Open only the home you need.

| home | owns |
| --- | --- |
| register.md | facts, KPIs, citations |
| thesis.md | working thesis and killing conditions |
| model.md | workbook pointer and reconciliation notes |
| consensus.md | what is already priced |

Do not duplicate a fact across homes. Point instead.
```

## Write rules

- Flush before context dies. A fact that lives only in this chat is lost.
- Mark material numbers with source and as-of, or write `not obtained`.
- Class facts when it helps: `[FACT]` (primary document + citation), `[DEDUCTED]` (computed from named inputs), `[VIEW]` (judgment).
- Do not write a prose diary. Registers and pointers, not a recap of the day.
- Do not store coverage facts in a bot's learning notes, in `book.db`, or in Ship's `factory.db`.
- Do not share one memory tree across two names.
- A declined name keeps its name id and tree. A new researcher on a later take-under-coverage reads that tree before continuing coverage.

## Do not

- Do not invent a second memory root
- Do not keep the node map in a sector bot — there isn't one; it lives in this name's register and thesis
- Do not flush secrets into memory
