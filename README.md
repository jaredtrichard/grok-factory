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

Bots never execute on the captain's computer. They run on the shared Grok Bot computer. Cursor cloud is for software work, the research scan swarm, and name research, models, and review.

After install, talk only to Firstmate.

## Features

- **One follow** — this pack. Software, research, and general-purpose.
- **Scan** — one scanning bot calls about ten Cursor cloud agents. Each returns its best money-making idea. Firstmate pitches only the winner, and only when it is strong.
- **Specify a name** — coverage for a name the captain already cares about. Skip the pitch. Take it under coverage.
- **Name coverage** — one active researcher per name for the life of coverage. If a discontinued name returns, a fresh researcher takes over. Three-statement model, in-depth research, a thesis the captain merges or sends back, then ongoing coverage.
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

# The scanning bot calls about ten cloud agents.
# Firstmate brings back one pitch, or says none cleared.

> take it under coverage

# A name researcher builds the model and thesis in the
# equity-research repo. A PR comes back. You merge.

> cover Acme

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
    │              ├─ scanner ─► ~10 cloud ideas ─► one pitch
    │              └─ name researcher ─► cloud ─► model + thesis PR ─► you merge
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

## License

MIT — see [LICENSE](LICENSE).
