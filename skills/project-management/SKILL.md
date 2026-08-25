---
name: Research book
description: Use at Grok Factory research intake and whenever work is handed to the scanning bot or a name researcher.
---

# Research book

A local sqlite database is the research book. Chat is not the source of truth.

Software stays in Ship's `/home/box/agent-data/grok-ship/factory.db`. Do not write research rows there. Do not write software rows here.

## Database path

On the shared Grok Bot computer:

`/home/box/agent-data/grok-factory/book.db`

Create the parent directory if needed. Same path every time. Do not create this file on the captain's computer.

Beside the book:

- `/home/box/agent-data/grok-factory/reports/<task id>.md` — scan pitches
- `/home/box/agent-data/grok-factory/models/<name id>/` — three-statement workbook
- `/home/box/agent-data/grok-factory/theses/<task id>.md` — staged thesis
- `/home/box/agent-data/grok-factory/memory/<name id>/` — coverage memory (see Coverage memory)

Sqlite routes. Memory is the mind.

## Schema

```sql
CREATE TABLE IF NOT EXISTS scans (
  id TEXT PRIMARY KEY,
  title TEXT NOT NULL,
  winner_name_id TEXT,
  report_ref TEXT,
  created_at INTEGER NOT NULL,
  updated_at INTEGER
);

CREATE TABLE IF NOT EXISTS names (
  id TEXT PRIMARY KEY,
  ticker TEXT COLLATE NOCASE UNIQUE,
  name TEXT NOT NULL,
  researcher_id TEXT,
  stage TEXT NOT NULL,
  thesis_ref TEXT,
  scan_id TEXT,
  created_at INTEGER NOT NULL,
  updated_at INTEGER,
  CHECK (ticker IS NULL OR (ticker COLLATE BINARY = UPPER(TRIM(ticker)) AND ticker <> '')),
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
  kind TEXT NOT NULL,
  title TEXT NOT NULL,
  prompt TEXT NOT NULL,
  name_id TEXT,
  scan_id TEXT,
  status TEXT NOT NULL,
  gate_kind TEXT,
  gate_ref TEXT,
  result TEXT,
  created_at INTEGER NOT NULL,
  updated_at INTEGER
);
```

`names.stage` is `candidate`, `coverage`, `live`, or `declined`. There is no watch stage.

`names.ticker` is optional for scan candidates because not every money-making idea is a listed ticker. Coverage requires a ticker. Normalize it to uppercase with no surrounding whitespace before lookup or insert. A non-null ticker identifies one `names` row.

`names.thesis_ref` is the current published thesis path, or null. A staged file under `theses/` is not published until Firstmate sets this pointer after the captain approves.

`tasks.kind` is `scan`, `cover`, or `decision`.

`tasks.status` is `queued`, `underway`, `blocked`, `done`, or `cancelled`.

`tasks.result` is the outcome pointer: scan report path, or staged thesis path.

`gate_kind` is optional: `after-task`, `at-time`, or `captain`.

`scans.id` matches the `scan` task id. Research task ids use a `GF-` prefix.

The schema is deliberately minimal. Do not add execution, portfolio, or trade tables. Do not add a sector table.

## Setup

On Firstmate's first research intake, if `book.db` is missing, create it and run the schema above. If `book.db` exists, leave its schema and data untouched; this pack does not migrate existing books.

## Intake

Firstmate writes the task row before handing work off. Reuse that task id in the crewmate message. The prompt carries the goal, acceptance criteria, and constraints.

**Scan.** Insert a `scans` row and a `scan` task. Hand both to the one scanning bot. After the report lands, if it pitches a winner, normalize its ticker and look it up before inserting `names`. With no match, insert a row at stage `candidate` with `scan_id` set and `researcher_id` null. With a match, reuse that row: keep `candidate` as `candidate`; keep `coverage` or `live` and its `researcher_id`; keep `declined` as `declined`. Set `scans.winner_name_id` to the inserted or reused row. A `coverage` or `live` winner stays with its researcher; do not take it under coverage again. If the scan has no winner, do not insert a name.

**Specify a name.** Skip the scan and require a ticker. Normalize and look it up before signing on a researcher or inserting a row. With no match, sign on one fresh researcher and insert one `names` row with `stage` `coverage` and `researcher_id` set in that same insert. With a `candidate` or `declined` match, reuse the row and follow take-under-coverage. With a `coverage` or `live` match, reuse its `researcher_id`. Never insert a second row for the ticker.

**Take under coverage.** Require a ticker before assigning coverage. Normalize it and look it up again; if the ticker belongs to another row, use that row instead of creating or retickering this one. For `candidate`, sign on one fresh name researcher from `/home/box/agent-data/grok-factory/pack/GROK_BOT_RESEARCHER.md`, then set the ticker, `researcher_id`, and stage `coverage` together. For `declined`, sign on a fresh researcher, replace the retired `researcher_id`, and set stage `coverage`. For `coverage` or `live`, keep the stage and reuse its `researcher_id`; never sign on another researcher. File a `cover` task for the selected researcher. If the ticker is missing, a `candidate` already has a conflicting assignment, or a `coverage` or `live` row has a missing or conflicting assignment, block instead of creating another one.

**Discontinued.** Set stage `declined` and retire that agent. Reuse the same `names` row and memory tree. A later take-under-coverage gets a new agent.

**Thesis gate.** File a `decision` when the staged thesis needs approve or send-back. Publish by writing `names.thesis_ref` only after approve, then set stage `live`.

## Updates

The scanning bot or name researcher updates `status`, `result`, and `updated_at` as it works. Firstmate owns `names.stage`, `names.researcher_id`, and `names.thesis_ref`.

## Do not

- Do not keep the book only in chat
- Do not treat sqlite as coverage memory
- Do not file software work here
- Do not open a pull request from research
- Do not take a live trade
