# First Mate Lite

Instructions for setting up First Mate Lite on top of Grok Bot.
The user just needs to tell any bot in their Grok Bot: follow this file.

This file is an installer. Do not summarize.

First Mate Lite is a standalone pack. One Grok Bot. One Firstmate. No crew. Firstmate does intake, routing, and supervision at low reasoning effort, and hands substantial work to Cursor cloud agents. No equity research. No project crewmates.

## What you are installing

- A Firstmate the captain talks to from then on
- Global skills: Cloud agents, Adversarial review, Ahoy, Lavish session
- A local sqlite job log for cloud jobs
- An empty directory for scout reports

Do not create any other bot.

## The three computers

- The user's computer: their own machine. Bots never execute here.
- The shared Grok Bot computer: a persistent cloud VM that runs Firstmate. The job log, reviews, and lavish-axi run here.
- Cursor cloud agents: ephemeral cloud VMs that spin up on demand. Firstmate launches them for every substantial job.

## Files in this pack

Same directory as this file:

- `GROK_BOT_FIRSTMATE.md` — Firstmate charter
- `skills/cloud-agents/SKILL.md`
- `skills/adversarial-review/SKILL.md`
- `skills/ahoy/SKILL.md`
- `skills/lavish-session/SKILL.md`

## Steps

1. Copy this directory to `/home/box/agent-data/first-mate-lite/pack/` on the shared computer (clone or download it first if you only have this file's text). Every later reference to a pack file means that path. If a copy is already there, refresh it.

2. Create `/home/box/agent-data/first-mate-lite/reports/` if it does not exist. Do not seed files into it.

3. Look at the existing roster (agent profile folders). If a Firstmate already exists, reuse it. Do not create a second.

4. Read `GROK_BOT_FIRSTMATE.md`. If a Firstmate exists, replace that agent's description with it. Otherwise, CreateAgent name `Firstmate` with that description. If you are already Firstmate, keep your name and update your own description instead of cloning yourself.

5. Set Firstmate's reasoning effort to low if Grok Bot exposes that setting. If you cannot set it, tell the user where to set it.

6. Write four global workflows from the skill files. Names:
   - Cloud agents
   - Adversarial review
   - Ahoy
   - Lavish session
   Use each skill's description line as the workflow description. Do not install extra plugins without a yes from the user.

7. Create the job log with the Cloud agents skill if it does not exist. Path is in that skill.

8. Check for lavish-axi on the shared computer. Minimum version 0.1.53. If missing, run `npx -y lavish-axi@latest` or ask the user to install it. Session URLs are served from the shared computer and the user views them from their own computer, so confirm with the user that they can reach it (tailnet or exposed address). Do not pretend the live loop works without it.

9. Detect source control CLIs on the shared computer: `gh`, `glab`, Bitbucket, or Cursor Origin, and verify the matching CLI is authenticated - adversarial review reads branches through it. Do not assume GitHub. Cloud agents need the user's Cursor account connected to their forge. Ask the user to connect whatever is missing. Do not ask them to paste a token in chat.

10. If crew bots from an earlier pack exist (project crewmates, a scanning bot, name researchers), leave them alone and tell the user they are no longer used and can be deleted from the sidebar (right-click the row, Delete). Do not delete them yourself.

11. Message Firstmate with ready-id `FML-READY`. Tell it the pack path, the reports directory, and the `jobs.db` path, and to reply ready against `FML-READY` and leave a greeting for the captain.

12. Tell the user: talk only to Firstmate from here. If this starter bot is not Firstmate, it is leftover. They can delete it from the sidebar. You cannot delete it yourself.
