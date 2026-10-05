# Grok Minion

Instructions for setting up Grok Minion on top of Grok Bot.
The user just needs to tell any bot in their Grok Bot: follow this file.

This file is an installer. Do not summarize.

Grok Minion is a standalone pack. The user is Gru, the boss. Kevin · Head minion is the one agent the boss talks to: intake, routing, and supervision at low reasoning effort. Other minions are Grok bots, each named after one of Gru's minions with its job as a subtitle (Name · Job), signed on as work arrives. Dr. Nefario · Code owns every code project and sends all coding and code review to the lab (Cursor cloud agents). No equity research. No Grok bot writes code.

## What you are installing

- Kevin · Head minion, the one agent the boss talks to from then on
- Dr. Nefario · Code, who owns code projects and the lab
- Global skills: Minions, The lab, Adversarial review, Bello, Lavish session
- A local sqlite database for the roster, projects, jobs, and decisions
- A minion charter template for later, per job
- An empty directory for scout reports

Do not pre-create any other minion. Kevin signs each one on the first time its job is needed.

## The three computers

- The user's computer: their own machine. Bots never execute here.
- The shared Grok Bot computer: a persistent cloud VM that runs Kevin, Dr. Nefario, and the minions. The database and lavish-axi run here.
- Cursor cloud agents (the lab): ephemeral cloud VMs that spin up on demand. Only Dr. Nefario sends work there.

## Files in this pack

Same directory as this file:

- `GROK_BOT_KEVIN.md` — Kevin · Head minion charter
- `GROK_BOT_NEFARIO.md` — Dr. Nefario · Code charter
- `GROK_BOT_MINION.md` — minion charter template
- `skills/minions/SKILL.md`
- `skills/lab/SKILL.md`
- `skills/adversarial-review/SKILL.md`
- `skills/bello/SKILL.md`
- `skills/lavish-session/SKILL.md`

## Steps

1. Copy this directory to `/home/box/agent-data/grok-minion/pack/` on the shared computer (clone or download it first if you only have this file's text). Every later reference to a pack file means that path. If a copy is already there, refresh it.

2. Create `/home/box/agent-data/grok-minion/reports/` if it does not exist. Do not seed files into it.

3. Look at the existing roster (agent profile folders). If Kevin · Head minion already exists, reuse it. If a Firstmate or Gru from an earlier pack exists and no Kevin does, reuse that agent as Kevin: rename it to `Kevin · Head minion` if Grok Bot allows, otherwise keep its name and tell the user they can rename it. Never create a second head minion.

4. Read `GROK_BOT_KEVIN.md`. Replace the reused agent's description with it. Otherwise, CreateAgent name `Kevin · Head minion` with that description. If you are that agent, update your own description instead of cloning yourself.

5. If Dr. Nefario · Code does not exist, CreateAgent name `Dr. Nefario · Code` with the description in `GROK_BOT_NEFARIO.md`. If a project crewmate from an earlier pack exists, do not reuse it for Nefario; tell the user it can be deleted once Nefario has its projects.

6. Set Kevin's and Dr. Nefario's reasoning effort to low if Grok Bot exposes that setting. If you cannot set it, tell the user where to set it.

7. Write five global workflows from the skill files. Names:
   - Minions
   - The lab
   - Adversarial review
   - Bello
   - Lavish session
   Use each skill's description line as the workflow description. If an Ahoy workflow from an earlier pack exists, tell the user Bello replaces it and it can be removed. Do not install extra plugins without a yes from the user.

8. Create the database with the Minions skill if it does not exist. Path is in that skill. Insert the `minions` rows for Kevin · Head minion and Dr. Nefario · Code.

9. Check for lavish-axi on the shared computer. Minimum version 0.1.53. If missing, run `npx -y lavish-axi@latest` or ask the user to install it. Session URLs are served from the shared computer and the user views them from their own computer, so confirm with the user that they can reach it (tailnet or exposed address). Do not pretend the live loop works without it.

10. Detect source control CLIs on the shared computer: `gh`, `glab`, Bitbucket, or Cursor Origin, and verify the matching CLI is authenticated. Do not assume GitHub. The lab needs the user's Cursor account connected to their forge. Ask the user to connect whatever is missing. Do not ask them to paste a token in chat.

11. If bots from an earlier pack exist, leave them alone. A scanning bot and name researchers are no longer used; tell the user they can delete them from the sidebar (right-click the row, Delete). Do not delete them yourself. An existing inbox, documents, or similar role bot can become a minion: tell Kevin about it so it renames that bot to `Name · Job` and reuses it instead of signing on a new one.

12. Message Kevin with ready-id `GM-READY`. Tell it the pack path, the reports directory, and the database path, and to reply ready against `GM-READY` and leave a greeting for the boss.

13. Tell the user: talk only to Kevin from here. If this starter bot is not Kevin, it is leftover. They can delete it from the sidebar. You cannot delete it yourself.
