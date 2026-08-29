# Grok Factory

Instructions for setting up Grok Factory on top of Grok Bot.
The user just needs to tell any bot in their Grok Bot: follow this file.

This file is an installer. Do not summarize.

Grok Factory is a standalone pack. One Grok Bot. One Firstmate. Software, research, and everything else. Software `ship` means a PR. Research `cover` updates a name in the equity-research GitHub repo; the PR is how those files land.

## What you are installing

- A Firstmate the captain talks to from then on
- Global skills: lavish-session, adversarial-review, project-management, ahoy, Research book, Coverage memory
- A local sqlite database for software projects and tasks
- A crewmate template for later, per software project
- A scanning-bot template and a name-researcher template
- Empty directories for scan pitches and software scout reports
- A research book at `/home/box/agent-data/grok-factory/book.db`, initialized by Firstmate on first research intake

Do not pre-create name researchers or project crewmates. Firstmate signs on the scanning bot on ready if none exists.

## The three computers

- The user's computer: their own machine. Bots never execute here.
- The shared Grok Bot computer: a persistent cloud VM that runs all agents. Everything a bot runs - checks, both databases, reviews, lavish-axi - runs here.
- Cursor cloud agents: ephemeral cloud VMs that spin up on demand. Software project crewmates, the scanning bot, and name researchers call them. Firstmate does not. Cursor cloud does information gathering, model building, and research. Grok bots synthesize and write the reports.

## Files in this pack

Same directory as this file:

- `GROK_BOT_FIRSTMATE.md` — Firstmate charter
- `GROK_BOT_CREWMATE.md` — per-project software crewmate charter
- `GROK_BOT_SCANNER.md` — scanning-bot charter
- `GROK_BOT_RESEARCHER.md` — name-researcher charter
- `skills/lavish-session/SKILL.md`
- `skills/adversarial-review/SKILL.md`
- `skills/project-management/SKILL.md`
- `skills/ahoy/SKILL.md`
- `skills/research-book/SKILL.md`
- `skills/memory/SKILL.md`

## Steps

1. Copy this whole pack to `/home/box/agent-data/grok-factory/pack/` on the shared computer (clone or download it first if you only have this file's text). Every later reference to a pack file means that path. If a copy is already there, refresh it.

2. Create these empty directories if they do not exist. Do not seed files into them:
   - `/home/box/agent-data/grok-factory/reports/`
   - `/home/box/agent-data/grok-factory/scout-reports/`

3. Look at the existing roster (agent profile folders). If a Firstmate already exists, reuse it. Do not create a second.

4. Read `GROK_BOT_FIRSTMATE.md`. If a Firstmate exists, update that agent's description to it. Otherwise, CreateAgent name `Firstmate` with that description. If you are already Firstmate, keep your name and update your own description instead of cloning yourself.

5. Write six global workflows from the skill files. Names:
   - Lavish session
   - Adversarial review
   - Project management
   - Ahoy
   - Research book
   - Coverage memory
   Use each skill's description line as the workflow description. Do not install extra plugins without a yes from the user.

6. Run the project-management setup: create the sqlite DB if it does not exist. Path is in that skill. Same path every time.

7. Check for lavish-axi on the shared computer. Minimum version 0.1.53. If missing, run `npx -y lavish-axi@latest` or ask the user to install it. Session URLs are served from the shared computer and the user views them from their own computer, so confirm with the user that they can reach it (tailnet or exposed address). Do not pretend the live loop works without it.

8. Detect source control CLIs for software on the shared computer: `gh`, `glab`, Bitbucket, or Cursor Origin, and verify the matching CLI is authenticated - adversarial review reads software branches through it. Do not assume GitHub for software. Separately require authenticated `gh` access for the equity-research GitHub repo and its cover PRs. Cloud agents need the user's Cursor account connected to the software forge and to GitHub for research. Ask the user to connect whatever is missing. Do not ask them to paste a token in chat.

9. Do not create name researchers now. Message Firstmate with ready-id `GF-READY`. Tell it:
   - the pack path, the two empty directories, and the `factory.db` path
   - it must initialize `/home/box/agent-data/grok-factory/book.db` with the Research book skill on first research intake
   - it must take one decision card for the equity-research GitHub repo on first research intake if `/home/box/agent-data/grok-factory/research-remote` is missing, verify with authenticated `gh` that `viewerPermission` for the chosen `owner/name` is `ADMIN`, `MAINTAIN`, or `WRITE`, then write it
   - it must sign on one scanning bot from `GROK_BOT_SCANNER.md` if none exists
   - to reply ready against `GF-READY` and leave a greeting for the captain

10. Tell the user: talk only to Firstmate from here. This starter bot is leftover. They can delete it from the sidebar (right-click the row, Delete). You cannot delete it yourself.
