# Gru

A Grok Bot pack for running a solo business and a life: one Gru, a few role minions, and Dr. Nefario's lab for code.

## What it is

Gru is the one bot you talk to. His job is intake, routing, and supervision, so he runs at low reasoning effort.

- **Minions** are persistent Grok bots, one role each, signed on the first time that role is needed:
  - Kevin (lead minion): email and calendar
  - Stuart: website
  - Bob: social media
  - Dave: money
  - Jerry: files
- **The lab** is Cursor cloud agents (grok 4.6, high reasoning). All code changes go there, including the website's code, plus any heavy lifting. Only Gru sends work to the lab.
- **Dr. Nefario** is a fresh review subagent that inspects every code branch before a pull request. You merge.

Minions draft; you approve. Sending email, posting, paying, deleting or sharing files, and publishing to the live site each need your yes, unless you gave a standing approval for that kind of action.

Compared with the full Grok Factory pack in the repo root:

| | Grok Factory | Gru |
|---|---|---|
| Equity research (scan, cover, book) | yes | no |
| Grok bots that manage code | yes, one crewmate per project | no, code goes to the lab |
| Role bots | inbox, documents, as needed | admin, website, social, money, files |
| Cursor cloud agents | via crewmates and researchers | the lab, sent by Gru |
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
> what's on my calendar this week, and anything urgent in email?

# Kevin checks and reports. Replies come back as drafts for your yes.

> post the launch announcement on LinkedIn and X

# Bob drafts both. Gru brings them to you to approve.

> the contact form on the site is broken

# Code change, so it goes to the lab. Dr. Nefario reviews the branch,
# a PR comes back green. You merge.

> bello

# Recap of what happened since you last spoke, plus open decisions.
```

## How it works

```
 boss
   │  asks, approvals, "merge it"
   ▼
 Gru (low effort) ── jobs.db
   ├─ Kevin · Stuart · Bob · Dave · Jerry ─► drafts and results ─► your yes for anything outward
   └─ the lab (Cursor cloud) ─► report, or branch ─► Dr. Nefario review ─► PR ─► you merge
```

On the shared computer:

- `/home/box/agent-data/gru/jobs.db` (minion roster and job log)
- `/home/box/agent-data/gru/reports/`
- `/home/box/agent-data/gru/workspace-repo` (repo for lab jobs with no natural repo)
