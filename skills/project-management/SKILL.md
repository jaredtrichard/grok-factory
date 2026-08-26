---
name: Project management
description: Use at Grok Factory intake and whenever work is handed to a crewmate.
---

# Project management

A local sqlite database is the factory backlog. Chat is not the source of truth.

Research is not this database. Scans, names, and cover tasks live in the Research book.

## Database path

On the shared Grok Bot computer:

`/home/box/agent-data/grok-factory/factory.db`

Create the parent directory if needed. Same path every time. Do not invent a second software database.
Scout reports live beside it in `/home/box/agent-data/grok-factory/scout-reports/`, one file per task id.

## Schema

```sql
CREATE TABLE IF NOT EXISTS projects (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  crewmate_id TEXT,
  repos TEXT NOT NULL,
  source_control TEXT,
  created_at INTEGER NOT NULL
);

CREATE TABLE IF NOT EXISTS tasks (
  id TEXT PRIMARY KEY,
  kind TEXT NOT NULL,
  title TEXT NOT NULL,
  prompt TEXT NOT NULL,
  project_id TEXT,
  repo TEXT,
  branch TEXT,
  status TEXT NOT NULL,
  gate_kind TEXT,
  gate_ref TEXT,
  result TEXT,
  created_at INTEGER NOT NULL,
  updated_at INTEGER
);
```

`projects.repos` is a JSON array of repo slugs or URLs. `projects.source_control` is `github`, `gitlab`, `bitbucket`, or `origin`.

`tasks.kind` is `scout`, `ship`, or `decision`.
`tasks.status` is `queued`, `underway`, `blocked`, `done`, or `cancelled`.
`tasks.result` is the outcome pointer: scout report path, repo-backed ship PR URL, or default-project ship artifact path.
`gate_kind` is optional: `after-task`, `at-time`, or `captain`.

The schema is deliberately minimal: enough to route work and find its results. Do not add tables speculatively.

## Setup

If `factory.db` does not exist, create it and run the schema. If it exists, do not migrate inventively. Report the path to Firstmate.

## Intake

Firstmate writes a task row before handing work off. Reuse the task id in the crewmate message. A good `prompt` states the goal, acceptance criteria, and constraints - enough to act on without coming back for basics.

If the work belongs to a repo that has no project row, sign on a crewmate from the crewmate template (`/home/box/agent-data/grok-factory/pack/GROK_BOT_CREWMATE.md`) and insert the project row (crewmate_id plus repos plus source_control).

If a project row already maps that repo to a crewmate, reuse that crewmate.

Non-software work files under the reserved `default` project row (repos `[]`, no source_control); create that row on first use. Assign that work to a matching plain-role bot, not a project crewmate.

## Promotion

When the captain authorizes implementation after a scout, do not open a duplicate task: flip the same row's kind to ship and hand it back to the assigned bot with the scout report as context. A repo-backed ship follows the branch, review, and PR flow. A `default`-project ship produces the requested artifact without source control, writes its artifact path to `result`, and closes the task.

## Updates

The assigned bot updates `status`, `result`, and `updated_at` as it goes, plus `branch` for repo-backed work. Done means `result` holds the pointer: scout report path, repo-backed ship PR URL, or default-project ship artifact path.

## Do not

- Do not keep the backlog only in chat
- Do not create one Firstmate per project
- Do not assume GitHub when recording `source_control`
- Do not file research scans, names, or cover tasks here
