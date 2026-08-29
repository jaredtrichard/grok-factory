---
name: Coverage memory
description: Use whenever a name researcher reads or writes durable facts about a name under coverage.
---

# Coverage memory

Memory is the mind. Sqlite routes. Chat is neither.

Each name has one home tree in the equity-research GitHub repo:

`memory/<name id>/`

Create the directory when that name is taken under coverage. Do not create it for a scan candidate that was never taken.

The repo is the durable store. Read and write these files on the cover branch. Do not keep a second tree on the shared computer.

## Read first

Open `INDEX.md` before any other memory file. It says which home owns which kind of fact. Then open only the home you need.

If `INDEX.md` is missing, write it, then continue.

## Homes

| file | owns |
| --- | --- |
| `INDEX.md` | map of homes; no facts |
| `research.md` | research file; segment drivers |
| `register.md` | facts, KPIs, citations, `not obtained` |
| `thesis.md` | working thesis and killing conditions |
| `model.md` | pointer to `models/<name id>/` (including valuation) and reconciliation notes |
| `consensus.md` | what is already priced; views around the name |
| `charts/` | thin chart set, only when the thesis needs a picture |

One home per fact. If a number lives in the workbook, `model.md` points at the line. Do not copy that number into `register.md` or `thesis.md`. The price target lives in `models/<name id>/valuation.md`; point at it.

`research.md` is gate one of initiation. No model without it. No valuation without the model. No thesis without the valuation.

`thesis.md` is the working mind. The cover PR is what Firstmate takes to the captain. `names.thesis_ref` is the published pointer after merge. Do not treat those three as interchangeable.

The node map lives in this name's coverage: who pays whom, where value sits, what would break it.

## INDEX.md

Keep it concise:

```
# <name> memory

Read this first. Open only the home you need.

| home | owns |
| --- | --- |
| research.md | research file; segment drivers |
| register.md | facts, KPIs, citations |
| thesis.md | working thesis and killing conditions |
| model.md | workbook pointer, including valuation |
| consensus.md | what is already priced |
| charts/ | thin charts, only if the thesis needs them |

Do not duplicate a fact across homes. Point instead.
```

## Write rules

- Flush before context dies. A fact that lives only in this chat is lost.
- Mark material numbers with source and as-of, or write `not obtained`.
- Class facts when it helps: `[FACT]` (primary document + citation), `[DEDUCTED]` (computed from named inputs), `[VIEW]` (judgment).
- Do not write a prose diary. Registers and pointers, not a recap of the day.
- Do not store coverage facts in a bot's learning notes or in `book.db`.
- Do not share one memory tree across two names.
- A declined name keeps its name id. If it was previously under coverage, it also keeps its researcher and tree; the same researcher reads that tree before resuming. If it was never covered, its first researcher creates the tree when coverage starts.

## Do not

- Do not invent a second memory root
- Do not flush secrets into memory
