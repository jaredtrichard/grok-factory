# Gru

Instructions for setting up Gru on top of Grok Bot.
The user just needs to tell any bot in their Grok Bot: follow this file.

This file is an installer. Do not summarize.

Gru is a standalone pack. One Grok Bot. One Gru. Gru does intake, routing, and supervision at low reasoning effort. Minions (persistent Grok bots with one role each: admin, website, social media, money, files) do the work. Dr. Nefario's lab (Cursor cloud agents) does code and heavy lifting. No equity research. No bot owns code.

## What you are installing

- Gru, the one agent the user (the boss) talks to from then on
- Global skills: Minions, The lab, Adversarial review, Bello, Lavish session
- A local sqlite roster and job log
- A minion template for later, per role
- An empty directory for scout reports

Do not pre-create minions. Gru signs each one on the first time its role is needed.

## The three computers

- The user's computer: their own machine. Bots never execute here.
- The shared Grok Bot computer: a persistent cloud VM that runs Gru and the minions. The job log, reviews, and lavish-axi run here.
- Cursor cloud agents (the lab): ephemeral cloud VMs that spin up on demand. Only Gru sends work there.

## Files in this pack

Same directory as this file:

- `GROK_BOT_GRU.md` — Gru charter
- `GROK_BOT_MINION.md` — minion charter template
- `skills/minions/SKILL.md`
- `skills/lab/SKILL.md`
- `skills/adversarial-review/SKILL.md`
- `skills/bello/SKILL.md`
- `skills/lavish-session/SKILL.md`

## Steps

1. Copy this directory to `/home/box/agent-data/gru/pack/` on the shared computer (clone or download it first if you only have this file's text). Every later reference to a pack file means that path. If a copy is already there, refresh it.

2. Create `/home/box/agent-data/gru/reports/` if it does not exist. Do not seed files into it.

3. Look at the existing roster (agent profile folders). If a Gru already exists, reuse it. If a Firstmate from an earlier pack exists and no Gru does, reuse that Firstmate as Gru: rename it to `Gru` if Grok Bot allows, otherwise keep its name and tell the user they can rename it. Never create a second orchestrator.

4. Read `GROK_BOT_GRU.md`. Replace the reused agent's description with it. Otherwise, CreateAgent name `Gru` with that description. If you are that agent, update your own description instead of cloning yourself.

5. Set Gru's reasoning effort to low if Grok Bot exposes that setting. If you cannot set it, tell the user where to set it.

6. Write five global workflows from the skill files. Names:
   - Minions
   - The lab
   - Adversarial review
   - Bello
   - Lavish session
   Use each skill's description line as the workflow description. If an Ahoy workflow from an earlier pack exists, tell the user Bello replaces it and it can be removed. Do not install extra plugins without a yes from the user.

7. Create the roster and job log with the Minions skill if it does not exist. Path is in that skill.

8. Check for lavish-axi on the shared computer. Minimum version 0.1.53. If missing, run `npx -y lavish-axi@latest` or ask the user to install it. Session URLs are served from the shared computer and the user views them from their own computer, so confirm with the user that they can reach it (tailnet or exposed address). Do not pretend the live loop works without it.

9. Detect source control CLIs on the shared computer: `gh`, `glab`, Bitbucket, or Cursor Origin, and verify the matching CLI is authenticated - adversarial review reads branches through it. Do not assume GitHub. The lab needs the user's Cursor account connected to their forge. Ask the user to connect whatever is missing. Do not ask them to paste a token in chat.

10. If bots from an earlier pack exist, leave them alone. Project crewmates, a scanning bot, and name researchers are no longer used; tell the user they can delete them from the sidebar (right-click the row, Delete). Do not delete them yourself. An existing inbox, documents, or similar role bot can become the matching minion: tell Gru about it so it reuses that bot instead of signing on a new one.

11. Message Gru with ready-id `GRU-READY`. Tell it the pack path, the reports directory, and the `jobs.db` path, and to reply ready against `GRU-READY` and leave a greeting for the boss.

12. Tell the user: talk only to Gru from here. If this starter bot is not Gru, it is leftover. They can delete it from the sidebar. You cannot delete it yourself.
