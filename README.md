<h1 align="center">Grok Factory</h1>
<p align="center">
  <a
    href="https://img.shields.io/badge/platform-Grok%20Bot-blue?style=flat-square"
    ><img
      alt="Platform"
      src="https://img.shields.io/badge/platform-Grok%20Bot-blue?style=flat-square"
  /></a>
</p>

<h3 align="center">Software, research, and general-purpose on Grok Bot.</h3>

## What it is

Grok Factory is a standalone Grok Bot pack.

One Grok Bot. One Firstmate. Software, research, and everything else:

- **Software** — A `ship` is a pull request.
- **Research** — Scan for a money-making idea, or cover a name. `cover` updates that name in the equity-research GitHub repo. The PR is how the files land.
- **Everything else** — scout or ship under the reserved default project.

Bots never execute on the captain's computer. They run on the shared Grok Bot computer. Cursor cloud does software work and the heavy research: gathering, model building, and research. Grok bots synthesize and write.

After install, talk only to Firstmate.

## Features

- **One follow** — this pack. Software, research, and general-purpose.
- **Scan** — one scanning bot runs quantitative screens and a thematic sweep on Cursor cloud. Firstmate pitches only the winner, and only when it is strong.
- **Specify a name** — coverage for a name the captain already cares about. Supply its ticker, skip the pitch, and take it under coverage.
- **Name coverage** — one researcher per name forever. If a discontinued name returns, the same researcher resumes it. Initiation is research file, segment three-statement model, valuation, then thesis — one PR, no captain checkpoint between gates. Then ongoing coverage.
- **Equity-research repo** — the durable store for research and models. Each name updates that GitHub repo. `book.db` routes.
- **Research book** — chat is not the source of truth. `book.db` routes scans, names, and tasks. Memory is the mind.
- **Software factory** — scout vs ship, per-project crewmates, adversarial review before any software pull request, local sqlite backlog. You merge.

## Quick Start

Tell any Grok Bot:

```
follow https://github.com/jaredtrichard/grok-factory/blob/main/GROK_FACTORY.md
```

That installs this pack on the shared computer and hands you over to Firstmate.
Talk only to Firstmate from then on.

```
> scan for a money-making idea

# The scanning bot runs screens and a thematic sweep.
# Firstmate brings back one pitch, or says none cleared.

> take it under coverage

# A name researcher runs initiation and opens one PR
# on the equity-research repo. You merge.

> cover Acme, ticker ACME

# Skip the scan. Straight under coverage.

> fix the flaky login test on xyz

# Software path. A PR comes back. You merge.
```

## How it works

```
            captain
                  │  work, decisions, "merge it"
                  ▼
 ┌─────────────────────────────────────────┐
 │ Grok Factory                            │
 │ one Firstmate · factory.db · book.db    │
 └──┬──────────────┬───────────────────┬───┘
    │              │                   │
    ▼              ▼                   ▼
 software       research          default project
    │              │
    │              ├─ scanner ─► screens + sweep ─► one pitch
    │              └─ name researcher ─► cloud ─► research + model + valuation + thesis PR ─► you merge
    │
    └─ project crewmate ─► cloud ─► review ─► PR ─► you merge
```

Software stays in `factory.db`. Research lives in this pack's book. Non-software, non-research work files under the reserved `default` project as scout or ship.

On the shared computer:

- `/home/box/agent-data/grok-factory/factory.db`
- `/home/box/agent-data/grok-factory/book.db`
- `/home/box/agent-data/grok-factory/research-remote`
- `/home/box/agent-data/grok-factory/reports/`
- `/home/box/agent-data/grok-factory/scout-reports/`

Research and models live in the equity-research GitHub repo:

- `models/<name id>/`
- `memory/<name id>/`
