---
name: Minions
description: Use at Kevin's intake, whenever work is handed to a minion, when signing on a minion, and when logging a decision for the boss.
---

# Minions

Minions are persistent Grok bots on the shared computer, each with one job. Kevin routes; minions do the work. A small sqlite database is the roster, the job log, and the decision log. Chat is not the source of truth.

## Database

On the shared Grok Bot computer:

`/home/box/agent-data/grok-minion/minions.db`

Create the parent directory if needed. Same path every time. Do not invent a second database.

```sql
CREATE TABLE IF NOT EXISTS minions (
  name TEXT PRIMARY KEY,
  job TEXT NOT NULL,
  agent_id TEXT NOT NULL,
  created_at INTEGER NOT NULL
);

CREATE TABLE IF NOT EXISTS projects (
  id TEXT PRIMARY KEY,
  repos TEXT NOT NULL,
  source_control TEXT,
  created_at INTEGER NOT NULL
);

CREATE TABLE IF NOT EXISTS jobs (
  id TEXT PRIMARY KEY,
  kind TEXT NOT NULL,
  title TEXT NOT NULL,
  prompt TEXT NOT NULL,
  owner TEXT NOT NULL,
  project_id TEXT,
  branch TEXT,
  cloud_agent_id TEXT,
  status TEXT NOT NULL,
  result TEXT,
  created_at INTEGER NOT NULL,
  updated_at INTEGER
);
```

`minions.name` is the display name, `Name · Job` (for example `Stuart · Inbox`). `jobs.owner` is a minion name, or `Kevin` for a decision. `kind` is `scout`, `ship`, or `decision`. `status` is `queued`, `underway`, `blocked`, `done`, or `cancelled`. `projects.repos` is a JSON array of repo slugs or URLs; `projects.source_control` is `github`, `gitlab`, `bitbucket`, or `origin`. `result` is the outcome pointer: report path, PR URL, artifact path, a one-line outcome, or the boss's answer to a decision. Job ids use a `GM-` prefix.

If `minions.db` does not exist, create it and run the schema. If it exists, do not migrate inventively.

## Roster

Kevin · Head minion and Dr. Nefario · Code exist from install. Every other minion is signed on the first time work arrives that no existing minion's job covers. Jobs follow what the boss actually brings. Examples: inbox and calendar, website content, social media, money, files. These are examples, not a preset crew. Do not pre-create minions.

Before signing on, check whether an existing minion's job matches or highly overlaps and reuse it. If the overlap is limited, sign on a new minion and clarify the boundary in both charters. Code never gets its own minion: it belongs to Dr. Nefario.

Every new minion takes the next unused name of one of Gru's minions: Stuart, Bob, Dave, Jerry, Carl, Phil, Tim, Mark, Norbert, Jorge, Otto, then any other. Its display name is that name plus its job, `Name · Job`.

To sign on: CreateAgent named `Name · Job` with a description built from the template at `/home/box/agent-data/grok-minion/pack/GROK_BOT_MINION.md`, filling in the job section. Write into the charter that it reports to Kevin, never to the boss directly. Insert the `minions` row in the same step.

## Intake

Kevin writes the job row before handing work off. Reuse the job id in the message to the minion. A good `prompt` states the goal, acceptance criteria, and constraints - enough to act on without coming back for basics.

Scout is investigation, planning, or audit; the deliverable is a report or a one-line answer. Ship is an authorized change; the deliverable is the change itself. When the boss authorizes action after a scout, promote the same job (flip its kind to ship) rather than opening a duplicate.

## Decisions

Before Kevin sends a decision card, he writes a `decision` job at `blocked` with the question in `prompt`. When the boss answers, Kevin writes the answer to `result`, marks it `done` with `updated_at`, then acts. A skipped card counts as declined; record that.

## Updates

The owning minion updates `status`, `result`, and `updated_at` as it goes and reports to Kevin against the job id. Done means `result` holds the pointer.

## Do not

- Do not keep the job log only in chat
- Do not create a second head minion
- Do not sign on a minion for equity research or for code
