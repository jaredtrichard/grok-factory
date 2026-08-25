# Grok Factory

Instructions for setting up Grok Factory on top of Grok Bot.
The user just needs to tell any bot in their Grok Bot: follow this file.

This file is an installer. Do not summarize.

Grok Factory sits on Grok Ship and adds a research domain. One Grok Bot. One Firstmate. Three paths: software (Ship), research (this pack), general-purpose (Ship default). Software `ship` means a PR. Research does not open PRs.

## What you are installing

- Grok Ship, if it is not already on this computer (software factory + general-purpose)
- This research domain
- One Firstmate the captain talks to from then on (reuse Ship's; do not create a second)
- A scanning-bot template and a name-researcher template
- Research skills: Research book, Coverage memory
- Empty directories for scan pitches, models, staged theses, and per-name memory
- A research book at `/home/box/agent-data/grok-factory/book.db`, initialized by Firstmate on first research intake

Do not pre-create name researchers. Firstmate signs on the scanning bot on ready if none exists.

## The three computers

Same split as Ship. Do not invent a fourth.

- The captain's computer: their own machine. Bots never execute here.
- The shared Grok Bot computer: the persistent cloud VM. Every bot, both databases, reviews, browser work, EDGAR, and lavish-axi run here.
- Cursor cloud agents: ephemeral VMs. Ship uses them for software work. This pack uses them for the scan swarm only. Name coverage does not call cloud.

## Files in this pack

Same directory as this file:

- `GROK_BOT_FIRSTMATE.md` — Firstmate charter (factory: software + research + general-purpose)
- `GROK_BOT_SCANNER.md` — scanning-bot charter
- `GROK_BOT_RESEARCHER.md` — name-researcher charter
- `skills/project-management/SKILL.md` — research book
- `skills/memory/SKILL.md` — coverage memory

Ship files stay in Ship. Do not copy them here. Pointer: https://github.com/kunchenguid/grok-ship

## Steps

1. If `/home/box/agent-data/grok-ship/pack/GROK_SHIP.md` is missing, follow https://github.com/kunchenguid/grok-ship/blob/main/GROK_SHIP.md first, then return here and continue. If Ship is already installed, leave its pack, `factory.db`, skills, and crewmates in place.

2. Copy this whole pack to `/home/box/agent-data/grok-factory/pack/` on the shared computer (clone or download it first if you only have this file's text). Every later reference to a factory pack file means that path. If a copy is already there, refresh it. Do not touch `/home/box/agent-data/grok-ship/`.

3. Create these empty directories if they do not exist. Do not seed files into them:
   - `/home/box/agent-data/grok-factory/reports/`
   - `/home/box/agent-data/grok-factory/models/`
   - `/home/box/agent-data/grok-factory/theses/`
   - `/home/box/agent-data/grok-factory/memory/`

4. Look at the existing roster. If a Firstmate already exists, reuse it. Do not create a second.

5. Read `GROK_BOT_FIRSTMATE.md`. CreateAgent name `Firstmate` with that description, or update the existing Firstmate's description to it. If you are already Firstmate, keep your name and update your description instead of cloning yourself.

6. Write two global workflows from this pack's skill files. Names:
   - Research book
   - Coverage memory
   Use each skill's description line as the workflow description. Do not overwrite Ship's workflows (Project management, Adversarial review, Lavish session, Ahoy). Do not install extra plugins without a yes from the captain. Do not copy Ship's adversarial-review skill into this pack; software review stays on Ship. Research cover self-reviews.

7. Do not create name researchers now. Message Firstmate with ready-id `GF-READY`. Tell it:
   - Ship is installed; software stays in `/home/box/agent-data/grok-ship/factory.db`
   - this domain's pack path and the four empty directories
   - it must initialize `/home/box/agent-data/grok-factory/book.db` with the Research book skill on first research intake
   - it must sign on one scanning bot from `GROK_BOT_SCANNER.md` if none exists
   - to reply ready against `GF-READY` and leave a greeting for the captain

8. Tell the captain: talk only to Firstmate from here. This starter bot is leftover. They can delete it from the sidebar (right-click the row, Delete). You cannot delete it yourself.
