---
name: Research book
description: Use at Grok Factory research intake and whenever work is handed to the scanning bot or a name researcher.
---

# Research book

A local sqlite database is the research book. Chat is not the source of truth.

Software stays in `/home/box/agent-data/grok-factory/factory.db`. Do not write research rows there. Do not write software rows here.

The equity-research GitHub repo is the durable store for research and models. `book.db` routes. Each name updates that repo.

## Database path

On the shared Grok Bot computer:

`/home/box/agent-data/grok-factory/book.db`

Create the parent directory if needed. Same path every time. Do not create this file on the captain's computer.

The equity-research remote is a single line, `owner/name`, at:

`/home/box/agent-data/grok-factory/research-remote`

On first research intake, if that file is missing, Firstmate takes one decision card for the GitHub repo, then writes the file. Do not invent a slug.

Beside the book:

- `/home/box/agent-data/grok-factory/reports/<task id>.md` — scan pitches

Inside the equity-research repo (durable store):

- `models/<name id>/` — three-statement workbook
- `memory/<name id>/` — coverage memory (see Coverage memory)

Sqlite routes. Memory is the mind. The repo holds the mind.

## Schema

```sql
CREATE TABLE IF NOT EXISTS scans (
  id TEXT PRIMARY KEY,
  title TEXT NOT NULL,
  status TEXT NOT NULL CHECK (status IN ('queued', 'underway', 'blocked', 'done', 'cancelled')),
  winner_name_id TEXT,
  report_ref TEXT,
  created_at INTEGER NOT NULL,
  updated_at INTEGER
);

CREATE TABLE IF NOT EXISTS names (
  id TEXT PRIMARY KEY,
  ticker TEXT COLLATE NOCASE UNIQUE NOT NULL,
  name TEXT NOT NULL,
  researcher_id TEXT,
  stage TEXT NOT NULL CHECK (stage IN ('candidate', 'coverage', 'live', 'declined')),
  thesis_ref TEXT,
  scan_id TEXT,
  created_at INTEGER NOT NULL,
  updated_at INTEGER,
  CHECK (ticker COLLATE BINARY = UPPER(TRIM(ticker)) AND ticker <> ''),
  CHECK (
    stage NOT IN ('coverage', 'live') OR (
      ticker IS NOT NULL AND
      researcher_id IS NOT NULL AND
      TRIM(researcher_id) <> ''
    )
  )
);

CREATE TABLE IF NOT EXISTS tasks (
  id TEXT PRIMARY KEY,
  kind TEXT NOT NULL CHECK (kind IN ('scan', 'cover', 'decision')),
  title TEXT NOT NULL,
  prompt TEXT NOT NULL,
  name_id TEXT,
  scan_id TEXT,
  status TEXT NOT NULL CHECK (status IN ('queued', 'underway', 'blocked', 'done', 'cancelled')),
  gate_kind TEXT,
  gate_ref TEXT,
  result TEXT,
  created_at INTEGER NOT NULL,
  updated_at INTEGER
);
```

`names.stage` is `candidate`, `coverage`, `live`, or `declined`. There is no watch stage.

`names.ticker` is required. Normalize it to uppercase with no surrounding whitespace before lookup or insert. One ticker identifies one `names` row.

`names.thesis_ref` is the current published thesis path in the equity-research repo, or null. A branch is not published until the captain merges the cover PR and Firstmate sets this pointer.

`tasks.kind` is `scan`, `cover`, or `decision`.

`tasks.status` is `queued`, `underway`, `blocked`, `done`, or `cancelled`.

`tasks.result` is the outcome pointer: scan report path, or cover PR URL.

`gate_kind` is optional: `after-task`, `at-time`, or `captain`.

`scans.id` matches the `scan` task id. Research task ids use a `GF-` prefix.

The schema is deliberately minimal.

## Setup

On Firstmate's first research intake, if `book.db` is missing, create it and run the schema above. If `book.db` exists, leave its schema and data untouched; this pack does not migrate existing books.

If `research-remote` is missing, take one decision card for the equity-research GitHub repo. Require authenticated `gh` access and verify the chosen `owner/name` with `gh repo view` before writing it. If verification fails, leave the file missing and block research intake. Cloud agents need the captain's Cursor account connected to that GitHub.

## Intake

Firstmate writes the task row before handing work off. Reuse that task id in the crewmate message. The prompt carries the goal, acceptance criteria, and constraints.

**Scan.** Insert a `scans` row and a `scan` task, both at status `queued`. Hand both to the one scanning bot. A winner must have a normalized ticker. If the report claims a winner without one, do not pitch or insert it; return it to the scanner to supply the ticker or report no winner. For a valid winner, look up its ticker before inserting `names`. With no match, insert a row at stage `candidate` with `scan_id` set and `researcher_id` null. With a match, reuse that row: keep `candidate` as `candidate`; keep `coverage` or `live` and its `researcher_id`; keep `declined` as `declined`. Set `scans.winner_name_id` to the inserted or reused row. A `coverage` or `live` winner stays with its researcher; do not take it under coverage again. If the scan has no winner, do not insert a name.

**Specify a name.** Skip the scan and require a ticker. Normalize and look it up before signing on a researcher or inserting a row. With no match, sign on one fresh researcher and insert one `names` row with `stage` `coverage` and `researcher_id` set in that same insert. With a `candidate` or `declined` match, reuse the row and follow take-under-coverage. With a `coverage` or `live` match, reuse its `researcher_id`. Never insert a second row for the ticker.

**Take under coverage.** Require a ticker before assigning coverage. Normalize it and look it up again; if the ticker belongs to another row, use that row instead of creating or retickering this one. For `candidate`, sign on one fresh name researcher from `/home/box/agent-data/grok-factory/pack/GROK_BOT_RESEARCHER.md`, then set the ticker, `researcher_id`, and stage `coverage` together. For `declined`, sign on a fresh researcher, then set the normalized ticker, replace the retired `researcher_id`, and set stage `coverage` together. For `coverage` or `live`, keep the stage and reuse its `researcher_id`; never sign on another researcher. File a `cover` task for the selected researcher. If the ticker is missing, a `candidate` already has a conflicting assignment, or a `coverage` or `live` row has a missing or conflicting assignment, block instead of creating another one.

**Cover.** The researcher updates that name in the equity-research repo: branch, Cursor cloud review of the research and model, then a pull request. `cover` is the research verb. The PR is how the files land. Record the PR URL in `tasks.result`.

**Discontinued.** Set stage `declined` and retire that agent. Reuse the same `names` row and memory tree in the repo. A later take-under-coverage gets a new agent.

**Thesis gate.** The cover PR is the staged thesis. After the researcher confirms the captain-authorized merge landed, Firstmate publishes by writing `names.thesis_ref` to `memory/<name id>/thesis.md` in that repo, sets stage `live`, and marks the `cover` task `done` with `updated_at`. Send-back keeps the same task `underway`: add the captain's notes, then have the same researcher update the existing branch and PR. Do not create a replacement task or PR.

## Updates

The scanning bot or name researcher updates `status`, `result`, and `updated_at` as it works. Firstmate owns `names.stage`, `names.researcher_id`, and `names.thesis_ref`, and closes a `cover` task after its merge is confirmed.

## Do not

- Do not keep the book only in chat
- Do not treat sqlite as coverage memory
- Do not file software work here
- Do not treat a branch as published
