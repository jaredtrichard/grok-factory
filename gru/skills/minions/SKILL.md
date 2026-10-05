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

Roles are not fixed. Gru signs on a minion the first time work arrives that no existing minion's role covers, and gives it a plain role that fits the work the boss actually brings. Examples of roles: email and calendar admin, website upkeep, social media, finances, files. These are examples, not a preset crew. Do not pre-create minions.

Before signing on, check whether an existing minion's role matches or highly overlaps and reuse it. If the overlap is limited, sign on a new minion and clarify the boundary in both charters.

Names come from this list in order, then any other minion name: Kevin, Stuart, Bob, Dave, Jerry, Carl, Phil, Tim, Mark, Norbert. The first minion signed on is the lead minion: when the boss asks for an overview of their plate, Gru asks the lead minion first.

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
