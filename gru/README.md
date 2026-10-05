# Gru

A smaller Grok Bot pack: one Gru, no Grok crew, minions (Cursor cloud agents) for the real work.

## What it is

Gru is the only bot. His job is intake, routing, and supervision, so he runs at low reasoning effort. Anything substantial goes to a minion: a Cursor cloud agent (grok 4.6, high reasoning) named from the roster (Kevin, Stuart, Bob, ...). The minion comes back with a report or a branch. Dr. Nefario, a fresh review subagent, inspects every code branch before a pull request. You merge.

Compared with the full Grok Factory pack in the repo root:

| | Grok Factory | Gru |
|---|---|---|
| Equity research (scan, cover, book) | yes | no |
| Grok crewmates that manage code | yes, one per project | no |
| Cursor cloud agents | via crewmates and researchers | minions sent by Gru directly |
| Adversarial review before a PR | yes | yes (Dr. Nefario) |
| Backlog | `factory.db` + `book.db` | `jobs.db` |
| Voice | ship captain | Gru and the boss |

## Quick Start

Tell any Grok Bot:

```
follow https://github.com/jaredtrichard/grok-factory/blob/main/gru/GRU.md
```

Then talk only to Gru.

```
> why is the login test flaky on xyz?

# Scout. Kevin investigates on Cursor cloud. Gru brings back the report.

> fix it

# Same job promoted to ship, same minion. Kevin pushes a branch,
# Dr. Nefario reviews it, a PR comes back green. You merge.

> bello

# Recap of what happened since you last spoke, plus open decisions.
```

## How it works

```
 boss
    │  asks, decisions, "merge it"
    ▼
 Gru (low effort) ── jobs.db
    │
    └─ minion (Cursor cloud agent) ─► report, or branch ─► Dr. Nefario review ─► PR ─► you merge
```

On the shared computer:

- `/home/box/agent-data/gru/jobs.db`
- `/home/box/agent-data/gru/reports/`
- `/home/box/agent-data/gru/workspace-repo` (repo for jobs with no natural repo)
