# First Mate Lite

A smaller Grok Bot pack: one Firstmate, no crew, Cursor cloud agents for the real work.

## What it is

Firstmate is the only bot. Its job is intake, routing, and supervision, so it runs at low reasoning effort. Anything substantial goes to a Cursor cloud agent (grok 4.6, high reasoning), which does the investigation or the code and comes back with a report or a pull request. You merge.

Compared with the full Grok Factory pack in the repo root:

| | Grok Factory | First Mate Lite |
|---|---|---|
| Equity research (scan, cover, book) | yes | no |
| Grok crewmates that manage code | yes, one per project | no |
| Cursor cloud agents | via crewmates and researchers | launched by Firstmate directly |
| Adversarial review before a PR | yes | yes |
| Backlog | `factory.db` + `book.db` | `jobs.db` |

## Quick Start

Tell any Grok Bot:

```
follow https://github.com/jaredtrichard/grok-factory/blob/main/first-mate-lite/FIRST_MATE_LITE.md
```

Then talk only to Firstmate.

```
> why is the login test flaky on xyz?

# Scout. A cloud agent investigates. Firstmate brings back the report.

> fix it

# Same job promoted to ship. Cloud agent pushes a branch,
# adversarial review runs, a PR comes back green. You merge.
```

## How it works

```
 captain
    │  asks, decisions, "merge it"
    ▼
 Firstmate (low effort) ── jobs.db
    │
    └─ Cursor cloud agent ─► report, or branch ─► review ─► PR ─► you merge
```

On the shared computer:

- `/home/box/agent-data/first-mate-lite/jobs.db`
- `/home/box/agent-data/first-mate-lite/reports/`
- `/home/box/agent-data/first-mate-lite/workspace-repo` (repo for jobs with no natural repo)
