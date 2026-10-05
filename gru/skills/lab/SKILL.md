---
name: The lab
description: Use whenever Gru sends a job to the lab (a Cursor cloud agent), and on every wake while a lab job is underway.
---

# The lab

The lab is Dr. Nefario's: Cursor cloud agents, ephemeral VMs that do heavy work off the shared computer. Gru is the only one who sends work to the lab. All code changes go to the lab, never to a minion. A minion that needs heavy lifting or a code change asks Gru against its task id, and Gru files the lab job.

Chat is not the source of truth. The job log is.

## Job log

Lab jobs are rows in `jobs.db` with `owner` set to `lab`. The schema and path are in the Minions skill.

## Workspace repo

Every cloud job runs against a repo. Code work uses the repo it is about. A job with no natural repo (a non-code scout, a document) uses the workspace repo in `/home/box/agent-data/gru/workspace-repo`. If that file is missing on first need, take one decision card for which repo to use, check the boss's Cursor account can reach it, and write `owner/name` there.

## Launch

1. Write the job row first at `queued`, `owner` `lab`. A good `prompt` states the goal, acceptance criteria, and constraints - enough for the agent to act without coming back for basics.
2. Launch the cloud agent: grok 4.6, high reasoning, not fast. Include the job id in the agent's task.
3. Record `cloud_agent_id`, set `underway`, and tell the boss the job is under way in one line.

Scout: the agent investigates and returns a report. It does not push a fix. Save its final report to `/home/box/agent-data/gru/reports/<job id>.md` and record that path in `result`.

Ship (code): the agent implements on a branch, runs the project's tests, and pushes the branch. It does not open a pull request yet. Record `branch`. Then run the Adversarial review skill.

Ship (non-code): the agent produces the requested artifact in the workspace repo. Record its path in `result`.

## Follow-through

Check every `underway` lab job on each wake: read its cloud agent's state and update the row. If Grok Bot supports scheduled wakes, keep one standing wake while any lab job is underway and drop it when none are. A standing wake with nothing new stays quiet.

- Agent finished a scout: save the report, mark `done`, relay the finding to the boss, or to the minion that asked for it.
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
