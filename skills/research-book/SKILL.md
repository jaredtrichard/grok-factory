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

- `models/<name id>/` — three-statement workbook and valuation
- `memory/<name id>/` — coverage memory, including the research file and thesis (see Coverage memory)

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

`tasks.result` is the outcome pointer: scan report path, cover PR URL, or the captain's answer to a decision.

`gate_kind` is optional: `after-task`, `at-time`, or `captain`.

`scans.id` matches the `scan` task id. Research task ids use a `GF-` prefix.

The schema is deliberately minimal.

## Setup

On Firstmate's first research intake, if `book.db` is missing, create it and run the schema above. If `book.db` exists, leave its schema and data untouched; this pack does not migrate existing books.

If `research-remote` is missing, take one decision card for the equity-research GitHub repo. With authenticated `gh`, run `gh repo view <owner/name> --json viewerPermission --jq .viewerPermission`. Write `owner/name` only when the result is `ADMIN`, `MAINTAIN`, or `WRITE`. Otherwise leave the file missing and block research intake. Cloud agents need the captain's Cursor account connected to that GitHub.

## Intake

Firstmate writes the task row before handing work off. Reuse that task id in the crewmate message. The prompt carries the goal, acceptance criteria, and constraints.

**Decision.** Before showing any research choice card to the captain, insert a `decision` task with the exact question and options in `prompt`, status `blocked`, and `gate_kind` `captain`; set `name_id`, `scan_id`, and `gate_ref` when applicable. After the captain answers, write the answer to `result`, set status `done` with `updated_at`, then apply the decision. If the card is withdrawn without an answer, set the task `cancelled`.

**Scan.** Insert a `scans` row and a `scan` task, both at status `queued`. Hand both to the one scanning bot. The scanner runs quantitative screens and a thematic sweep, then pitches at most one tickered winner, or none. A winner must have a normalized ticker. If the report claims a winner without one, do not pitch or insert it; return it to the scanner to supply the ticker or report no winner. A valid pitch names the payer, the way to capture money, why it is strong, what others miss, key risks, and sources; required sections live in the scanning-bot charter. For a valid winner, look up its ticker before inserting `names`. With no match, insert a row at stage `candidate` with `scan_id` set and `researcher_id` null. With a match, reuse that row: keep `candidate` as `candidate`; keep `coverage` or `live` and its `researcher_id`; keep `declined` as `declined`. Set `scans.winner_name_id` to the inserted or reused row. A `coverage` or `live` winner stays with its researcher; do not take it under coverage again. If the scan has no winner, do not insert a name.

**Specify a name.** Skip the scan and require a ticker. Normalize and look it up before signing on a researcher or inserting a row. With no match, sign on one researcher and insert one `names` row with `stage` `coverage` and `researcher_id` set in that same insert. With a `candidate` or `declined` match, reuse the row and follow take-under-coverage. With a `coverage` or `live` match, reuse its `researcher_id`. Never insert a second row for the ticker.

**Take under coverage.** Require a ticker before assigning coverage. Normalize it and look it up again; if the ticker belongs to another row, use that row instead of creating or retickering this one. For `candidate` or `declined` with a null `researcher_id`, sign on one name researcher from `/home/box/agent-data/grok-factory/pack/GROK_BOT_RESEARCHER.md`, then set the ticker, `researcher_id`, and stage `coverage` together. If either stage already has a `researcher_id`, reuse it and set the normalized ticker and stage `coverage`. For `coverage` or `live`, keep the stage and reuse its `researcher_id`; block if that assignment is missing. Never replace a non-null `researcher_id` or sign on a second researcher for one name. File a `cover` task for the selected researcher through the per-name queue below. If the ticker is missing or conflicts with another row, block instead of creating another one. Firstmate does not checkpoint initiation gates with the captain.

**Cover.** Before inserting a cover, query that name's `queued`, `underway`, and `blocked` cover tasks. With none, insert the new cover at `queued` and hand it to the researcher; the researcher moves it to `underway`. With any nonterminal cover, insert the new cover at `queued` with `gate_kind` `after-task` and `gate_ref` set to the newest nonterminal cover id, and do not hand it off. Firstmate releases covers in order only after every earlier cover for the name is `done` or `cancelled`. Send-back revisions reuse the current cover task and PR rather than entering this queue. Initiation produces, in order: research file, segment three-statement model, valuation, thesis; Cursor cloud review of all four; then one pull request. Required sections for those artifacts, and for each ongoing cover mode, live in the name-researcher charter. Firstmate does not checkpoint them with the captain. Ongoing cover uses the same verb with named modes (print plug / model update, thesis scorecard, earnings preview) and a new PR only when the view moved. `cover` is the research verb. The PR is how the files land. Record the PR URL in `tasks.result`.

**Discontinued.** First have the researcher stop every in-flight cloud job for each `queued`, `underway`, or `blocked` `cover` task for that name and wait until every job is terminal. After the researcher confirms no job can still publish, have them recheck the equity-research repo, close every open PR for those tasks, and recheck that none remain. Only then set each task to `cancelled` with `updated_at`, set the name stage `declined`, and stand the researcher down. Keep the same `researcher_id`, `names` row, and memory tree in the repo. A later take-under-coverage returns to that researcher.

**Thesis gate.** The cover PR is the staged thesis. After the researcher confirms the captain-authorized merge landed, Firstmate publishes by writing `names.thesis_ref` to `memory/<name id>/thesis.md` in that repo, sets stage `live`, and marks the `cover` task `done` with `updated_at`. Send-back keeps the same task `underway`: add the captain's notes, then have the same researcher update the existing branch and PR. Do not create a replacement task or PR.

## Updates

The scanning bot or name researcher updates `status`, `result`, and `updated_at` as it works. Firstmate owns `names.stage`, `names.researcher_id`, and `names.thesis_ref`, closes a `cover` task after its merge is confirmed, releases the next queued cover only after earlier covers for that name are terminal, and cancels nonterminal cover tasks only after their cloud jobs are terminal and their PRs are rechecked closed on discontinuation.

## Do not

- Do not keep the book only in chat
- Do not treat sqlite as coverage memory
- Do not file software work here
- Do not treat a branch as published
