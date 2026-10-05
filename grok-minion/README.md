# Grok Minion

A general Grok Bot pack. You are Gru. Kevin is your head minion. Other minions take on whatever jobs you need, Dr. Nefario owns code, and the lab (Cursor cloud agents) does the coding and reviews everything before you see it.

## What it is

- **Kevin · Head minion** is the one bot you talk to. He takes every request, routes it, tracks it, and calls you boss.
- **Minions** are Grok bots, each named after one of Gru's minions with its job as a subtitle: `Stuart · Inbox`, `Bob · Website`, `Dave · Social`. Kevin signs one on the first time you bring work no minion covers. The jobs follow what you bring; those are just examples.
- **Dr. Nefario · Code** owns every code project. He never writes code himself.
- **The lab** is Cursor cloud agents on Auto, so Cursor picks the model and reasoning level for each task. It does all coding, and it reviews everything before it reaches you: code, writing, email or file deletions, finances, reports. The reviewer is always a fresh agent that did not do the work. Only trivial answers skip review.

Minions draft and you approve. Sending email, posting, paying, deleting or sharing files, and publishing to a live site each need your yes, unless you gave a standing OK for that kind of action.

Compared with the full Grok Factory pack in the repo root:

| | Grok Factory | Grok Minion |
|---|---|---|
| Equity research (scan, cover, book) | yes | no |
| Grok bots for code | one crewmate per project | Dr. Nefario for all projects |
| Review | code only, fresh subagent on the shared computer | everything non-trivial, fresh agent in the lab |
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
   ├─ Stuart · Inbox, Bob · Website, ... ─► drafts ─► lab review ─► your yes for anything outward
   └─ Dr. Nefario · Code ─► the lab: code agent ─► fresh review agent ─► PR ─► you merge
```

On the shared computer:

- `/home/box/agent-data/grok-minion/minions.db`
- `/home/box/agent-data/grok-minion/reports/`
- `/home/box/agent-data/grok-minion/workspace-repo` (repo for lab jobs with no natural repo)
