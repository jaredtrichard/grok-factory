<h1 align="center">Grok Factory</h1>
<p align="center">
  <a
    href="https://img.shields.io/badge/platform-Grok%20Bot-blue?style=flat-square"
    ><img
      alt="Platform"
      src="https://img.shields.io/badge/platform-Grok%20Bot-blue?style=flat-square"
  /></a>
  <a
    href="https://github.com/kunchenguid/grok-ship"
    ><img
      alt="OS"
      src="https://img.shields.io/badge/OS-Grok%20Ship-black?style=flat-square"
  /></a>
</p>

<h3 align="center">Software, research, and general-purpose on Grok Ship.</h3>

## What it is

Grok Factory is a Grok Bot distro that sits on [Grok Ship](https://github.com/kunchenguid/grok-ship) and adds a research domain.

One Grok Bot. One Firstmate. Three paths:

- **Software** — Ship. A `ship` is a pull request.
- **Research** — this pack. Scan for a money-making idea, or cover a name. Research does not open PRs.
- **General-purpose** — Ship's default / non-software project.

Bots never execute on the captain's computer. They run on the shared Grok Bot computer. Cursor cloud is for Ship software work and for the research scan swarm only.

After install, talk only to Firstmate. No bot takes a live trade.

## Features

- **One follow** — installs Ship plus this research domain.
- **Scan** — one scanning bot calls about ten Cursor cloud agents. Each returns its best money-making idea. Firstmate pitches only the winner, and only when it is strong.
- **Specify a name** — coverage for a name the captain already cares about. Skip the pitch. Take it under coverage.
- **Name coverage** — one researcher per name, forever. Three-statement model, in-depth research, a thesis the captain approves or sends back, then ongoing coverage.
- **Research book** — chat is not the source of truth. `book.db` routes scans, names, and tasks. Memory is the mind.
- **No live trades** — there is no exchange, brokerage, or order routing.

## Quick Start

Tell any Grok Bot:

```
follow https://github.com/jaredtrichard/grok-factory/blob/main/GROK_FACTORY.md
```

That installs Ship if needed, adds this domain, and hands you over to Firstmate.
Talk only to Firstmate from then on.

```
> scan for a money-making idea

# The scanning bot calls about ten cloud agents.
# Firstmate brings back one pitch, or says none cleared.

> take it under coverage

# A name researcher builds the model and thesis.
# You approve the thesis or send it back.

> cover Acme

# Skip the scan. Straight under coverage.

> fix the flaky login test on xyz

# Software path on Ship. A PR comes back. You merge.
```

## How it works

```
            captain
                  │  work, decisions, "merge it"
                  ▼
 ┌─────────────────────────────────────────┐
 │ Grok Factory                            │
 │ one Firstmate · Ship OS · research book │
 └──┬──────────────┬───────────────────┬───┘
    │              │                   │
    ▼              ▼                   ▼
 software       research          general-purpose
 (Ship)         (this pack)       (Ship default)
    │              │
    │              ├─ scanner ─► ~10 cloud ideas ─► one pitch
    │              └─ name researcher ─► model + thesis ─► you approve
    │
    └─ project crewmate ─► cloud ─► review ─► PR ─► you merge
```

Software stays in Ship's `factory.db`. Research lives in this pack's book. General-purpose files under Ship's reserved default project.

Research data on the shared computer:

- `/home/box/agent-data/grok-factory/book.db`
- `/home/box/agent-data/grok-factory/reports/`
- `/home/box/agent-data/grok-factory/models/<name id>/`
- `/home/box/agent-data/grok-factory/theses/`
- `/home/box/agent-data/grok-factory/memory/<name id>/`

## License

MIT — see [LICENSE](LICENSE).
