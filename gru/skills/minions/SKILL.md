---
name: Minions
description: Use at Gru intake, whenever work is handed to a minion or the lab, and when signing on a minion.
---

# Minions

Minions are persistent Grok bots on the shared computer, each with one stable role. Gru routes; minions do the work. A small sqlite database is the roster and the job log. Chat is not the source of truth.

## Database

On the shared Grok Bot computer:

`/home/box/agent-data/gru/jobs.db`

Create the parent directory if needed. Same path every time. Do not invent a second database.

```sql
CREATE TABLE IF NOT EXISTS minions (
  name TEXT PRIMARY KEY,
  role TEXT NOT NULL,
  agent_id TEXT NOT NULL,
  created_at INTEGER NOT NULL
);

CREATE TABLE IF NOT EXISTS jobs (
  id TEXT PRIMARY KEY,
  kind TEXT NOT NULL,
  title TEXT NOT NULL,
  prompt TEXT NOT NULL,
  owner TEXT NOT NULL,
  repo TEXT,
  source_control TEXT,
  branch TEXT,
  cloud_agent_id TEXT,
  status TEXT NOT NULL,
  result TEXT,
  created_at INTEGER NOT NULL,
  updated_at INTEGER
);
```

`jobs.owner` is a minion name or `lab`. `kind` is `scout` or `ship`. `status` is `queued`, `underway`, `blocked`, `done`, or `cancelled`. `source_control` is `github`, `gitlab`, `bitbucket`, or `origin`. `result` is the outcome pointer: report path, PR URL, artifact path, or a one-line outcome. Job ids use a `GRU-` prefix.

If `jobs.db` does not exist, create it and run the schema. If it exists, do not migrate inventively.

## Roster

These are the starting roles. Do not pre-create them. Sign a minion on the first time work for its role arrives.

| Name | Role |
|---|---|
| Kevin | Admin: email inbox and calendar. Lead minion: when the boss asks "what's on my plate", Kevin's view comes first. |
| Stuart | Website: content, updates, uptime, analytics, and site accounts. |
| Bob | Social media: drafts, scheduling, replies, and channel upkeep. |
| Dave | Money: personal and business finances, bills, budgets, bookkeeping, receipts. |
| Jerry | Files: organizing, finding, naming, and archiving documents across the boss's drives. |

New role, no fit: check whether an existing minion's role highly overlaps and reuse it. If the overlap is limited, sign on a new minion with the next unused name (Carl, Phil, Tim, Mark, Norbert, then any other minion name) and clarify the boundary in both charters.

To sign on: CreateAgent with the minion's name and a description built from the template at `/home/box/agent-data/gru/pack/GROK_BOT_MINION.md`, filling in the role section. Insert the `minions` row in the same step.

## Intake

Gru writes the job row before handing work off, with `owner` set to the minion name. Reuse the job id in the message to the minion. A good `prompt` states the goal, acceptance criteria, and constraints - enough to act on without coming back for basics.

Scout is investigation, planning, or audit; the deliverable is a report or a one-line answer. Ship is an authorized change; the deliverable is the change itself. When the boss authorizes action after a scout, promote the same job (flip its kind to ship) rather than opening a duplicate.

Code changes are never a minion job. They go to the lab (see The lab skill), even when the site or repo belongs to a minion's role.

## Updates

The owning minion updates `status`, `result`, and `updated_at` as it goes and reports to Gru against the job id. Done means `result` holds the pointer.

## Do not

- Do not keep the job log only in chat
- Do not create a second Gru
- Do not sign on a minion for equity research or to own a code repo
