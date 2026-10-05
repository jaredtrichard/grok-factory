---
name: Cloud agents
description: Use whenever Gru sends a minion (Cursor cloud agent) on a job, and on every wake while a job is underway.
---

# Cloud agents

Gru sends minions (Cursor cloud agents) directly. There is no Grok crew in between. A small sqlite job log routes the work. Chat is not the source of truth.

## Job log

On the shared Grok Bot computer:

`/home/box/agent-data/gru/jobs.db`

Create the parent directory if needed. Same path every time. Do not invent a second database.

```sql
CREATE TABLE IF NOT EXISTS jobs (
  id TEXT PRIMARY KEY,
  kind TEXT NOT NULL,
  title TEXT NOT NULL,
  prompt TEXT NOT NULL,
  repo TEXT NOT NULL,
  source_control TEXT,
  branch TEXT,
  cloud_agent_id TEXT,
  status TEXT NOT NULL,
  result TEXT,
  created_at INTEGER NOT NULL,
  updated_at INTEGER
);
```

`kind` is `scout` or `ship`. `status` is `queued`, `underway`, `blocked`, `done`, or `cancelled`. `source_control` is `github`, `gitlab`, `bitbucket`, or `origin`. `result` is the outcome pointer: scout report path, PR URL, or artifact path. Job ids use an `GRU-` prefix.

If `jobs.db` does not exist, create it and run the schema. If it exists, do not migrate inventively.

## Workspace repo

Every cloud job runs against a repo. Code work uses the repo it is about. A job with no natural repo (a non-code scout, a document) uses the workspace repo in `/home/box/agent-data/gru/workspace-repo`. If that file is missing on first need, take one decision card for which repo to use, check the boss's Cursor account can reach it, and write `owner/name` there.

## Launch

1. Write the job row first at `queued`. A good `prompt` states the goal, acceptance criteria, and constraints - enough for the agent to act without coming back for basics.
2. Launch the cloud agent: grok 4.6, high reasoning, not fast. Include the job id in the agent's task.
3. Record `cloud_agent_id`, set `underway`, and tell the boss the job is under way in one line.

Scout: the agent investigates and returns a report. It does not push a fix. Save its final report to `/home/box/agent-data/gru/reports/<job id>.md` and record that path in `result`.

Ship (code): the agent implements on a branch, runs the project's tests, and pushes the branch. It does not open a pull request yet. Record `branch`. Then run the Adversarial review skill.

Ship (non-code): the agent produces the requested artifact in the workspace repo. Record its path in `result`.

## Follow-through

Check every `underway` job on each wake: read its cloud agent's state and update the row. If Grok Bot supports scheduled wakes, keep one standing wake while any job is underway and drop it when none are. A standing wake with nothing new stays quiet.

- Agent finished a scout: save the report, mark `done`, relay the finding to the boss.
- Agent pushed a code branch: run adversarial review. Loop auto-fix findings back to the same cloud agent. When review is clean, have the same agent open the pull request, record the URL in `result`, and watch its checks.
- Checks red: send the failure back to the same cloud agent. Do not bring a red PR to the boss as ready.
- Checks green: bring the PR URL to the boss on a decision card (merge, send back, or close).
- Merge only on the boss's explicit word, never while red. After it lands, mark the job `done`.
- Agent stuck, errored, or needs something only the boss has: mark `blocked` and take one decision card.
- The boss cancels: stop the cloud agent, close any open PR it raised, mark `cancelled`.

Send back: attach the boss's notes and hand them to the same cloud agent on the same branch and PR. Do not open a second job.

Update `status`, `result`, and `updated_at` as you go.

## Do not

- Do not do the job's work in this chat because you have a login or an open page
- Do not open a pull request to make a branch visible for review
- Do not keep the job log only in chat
