# Grok Minion

A general Grok Bot pack. You are Gru. Kevin is your head minion. Other minions take on whatever jobs you need, Dr. Nefario owns code, and the lab (Cursor cloud agents) does the coding and reviews everything before you see it.

## What it is

- **Kevin · Head minion** is the one bot you talk to. He takes every request, routes it, tracks it, and calls you boss.
- **Minions** are Grok bots, shown as `Name · Job`. Kevin and Dr. Nefario are the only ones with preset jobs. Every other minion is signed on the first time you bring work no minion covers, gets a random name from Gru's minions, and takes that work as its job.
- **Dr. Nefario · Code** owns every code project. He never writes code himself.
- **The lab** is Cursor cloud agents on Auto, so Cursor picks the model and reasoning level for each task. It does all coding, and it reviews everything before it reaches you: code, writing (in your voice, scrubbed of AI-speak), email or file deletions, finances, reports. The reviewer is always a fresh agent that did not do the work. Only trivial answers skip review.

Minions draft and you approve. Drafts land where they will be sent (email drafts in your email, posts as drafts in Substack or the social app) so you edit them there. Sending, posting, paying, deleting or sharing files, and publishing to a live site each need your yes, unless you gave a standing OK. Clearly obvious cleanup, like marketing mail, can go after lab review.

- **The learning loop** keeps a profile of how you write and how you handle things (what you archive, what you edit out of drafts). Minions read it before they work and update it from what you change, so results get more relevant over time.
- **Project titles**: every message from Kevin about a piece of work starts with that project's title in bold, so you can follow each thread.

Compared with the full Grok Factory pack in the repo root:

| | Grok Factory | Grok Minion |
|---|---|---|
| Equity research (scan, cover, book) | yes | no |
| Grok bots for code | one crewmate per project | Dr. Nefario for all projects |
| Review | code only, fresh subagent on the shared computer | everything non-trivial, fresh agent in the lab |
| Other bots | inbox, documents, as needed | minions, any job, as needed |
| Backlog | `factory.db` + `book.db` | `minions.db` (roster, jobs, decisions) + a learned profile of you |
| Voice | ship captain | Kevin and the boss, light minionese |

## Quick Start

Tell any Grok Bot:

```
follow https://github.com/jaredtrichard/grok-factory/blob/main/grok-minion/GROK_MINION.md
```

Then talk only to Kevin.

```
> what's on my calendar this week, and anything urgent in email?

**Inbox this week**
Bello, boss! Went through 42 emails and archived 30 promos. Three
need you: the venue contract, a client invoice question, and
Thursday's dentist reschedule. Replies are waiting in your Gmail drafts.

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
   ├─ minions (Name · Job) ─► drafts in place ─► lab review ─► your edit or yes
   └─ Dr. Nefario · Code ─► the lab: code agent ─► fresh review agent ─► PR ─► you merge
```

On the shared computer:

- `/home/box/agent-data/grok-minion/minions.db`
- `/home/box/agent-data/grok-minion/reports/`
- `/home/box/agent-data/grok-minion/boss/` (the learned profile: voice, habits, preferences)
- `/home/box/agent-data/grok-minion/workspace-repo` (repo for lab jobs with no natural repo)
