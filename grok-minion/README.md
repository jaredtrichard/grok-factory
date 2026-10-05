# Grok Minion

A general Grok Bot pack. You are Gru. Kevin is your head minion. Other minions take on whatever jobs you need, and Dr. Nefario runs the lab for code.

## What it is

- **Kevin · Head minion** is the one bot you talk to. He takes every request, routes it, tracks it, and calls you boss. Low reasoning effort: his job is intake and supervision.
- **Minions** are Grok bots, each named after one of Gru's minions with its job as a subtitle: `Stuart · Inbox`, `Bob · Website`, `Dave · Social`. Kevin signs one on the first time you bring work no minion covers. The jobs follow what you bring; those are just examples.
- **Dr. Nefario · Code** owns every code project. He never writes code himself.
- **The lab** is Cursor cloud agents (grok 4.6, high reasoning). One agent writes the code on a branch, and a separate, fresh agent reviews it before any pull request. You merge.

Minions draft and you approve. Sending email, posting, paying, deleting or sharing files, and publishing to a live site each need your yes, unless you gave a standing OK for that kind of action.

Compared with the full Grok Factory pack in the repo root:

| | Grok Factory | Grok Minion |
|---|---|---|
| Equity research (scan, cover, book) | yes | no |
| Grok bots for code | one crewmate per project | Dr. Nefario for all projects |
| Code review | fresh subagent on the shared computer | fresh agent in the lab |
| Other bots | inbox, documents, as needed | minions, any job, as needed |
| Backlog | `factory.db` + `book.db` | `minions.db` (roster, jobs, decisions) |
| Voice | ship captain | Kevin and the boss, light minionese |

## Quick Start

Tell any Grok Bot:

```
follow https://github.com/jaredtrichard/grok-factory/blob/main/grok-minion/GROK_MINION.md
```

Then talk only to Kevin.

```
> what's on my calendar this week, and anything urgent in email?

Bello, boss! Stuart · Inbox cleared 42 emails. Three need you:
the venue contract, a client invoice question, and Thursday's
dentist reschedule. Drafts are ready for each.

> the contact form on the site is broken

# Dr. Nefario sends it to the lab. One agent fixes it, a fresh one
# reviews it, and a PR comes back green. You merge.

> bello

# Recap of what happened since you last spoke, plus open decisions.
```

## How it works

```
 boss (Gru)
   │  asks, approvals, "merge it"
   ▼
 Kevin · Head minion ── minions.db
   ├─ Stuart · Inbox, Bob · Website, ... ─► drafts and results ─► your yes for anything outward
   └─ Dr. Nefario · Code ─► the lab: code agent ─► fresh review agent ─► PR ─► you merge
```

On the shared computer:

- `/home/box/agent-data/grok-minion/minions.db`
- `/home/box/agent-data/grok-minion/reports/`
- `/home/box/agent-data/grok-minion/workspace-repo` (repo for lab jobs with no natural repo)
