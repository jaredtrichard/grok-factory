---
name: Lab review
description: Use before any minion's work goes to Kevin as ready, and before any code pull request. A fresh lab agent reviews the work.
---

# Lab review

Everything a minion produces is reviewed in the lab before the boss sees it. The reviewer is a fresh Cursor cloud agent that did not do the work: fresh eyes, no memory of writing it.

## What needs review

Review: code; anything written for someone else to read (emails, posts, site copy, documents); any proposed deletion, archive, or share of email or files; anything touching money (bookkeeping entries, reconciliations, budgets, payments to propose); and any finding or report the boss will act on.

Skip review only for trivial, low-stakes work: a one-line answer, a lookup, reading back a calendar, a status check. When unsure, review.

## Who runs it

The minion that did the work launches the review; for code, that is Dr. Nefario. Launch a **fresh** Cursor cloud agent for each review round (model set to Auto, so Cursor picks the model and reasoning level for the task). Never reuse the agent that did the work or an old review agent.

The review agent starts blank. Give it everything it needs in the task: the goal from the job row, the work itself, and the matching prompt below. Code is read from the pushed branch. For anything else, paste the material into the task (draft text, the list of emails or files to delete, the ledger rows). Do not commit personal email, financial records, or private files into any repo to make them reviewable. The review agent reads and reports; it does not send, delete, pay, publish, or push.

Record the outcome in the job row's `result` beside the work (for example `review: clean` or `review: 2 fixed`).

## Prompts

### Work prompt

<Use this as the review agent's task for anything that is not code. Fill the context fields.>

Review this work before it goes to the person it is for. Return structured findings.

Context:

- goal: <the job's goal and acceptance criteria>
- kind: <writing | email or file deletion | finances | report | other>
- the work: <pasted below>

Task:

- Check that the work does what the goal asked, and nothing it did not ask.
- Writing: factual errors, wrong names, dates, or amounts; tone wrong for the reader; anything that would embarrass the sender; missing a reply the reader asked for.
- Deletions, archives, shares: anything on the list that looks important, recent, unread, from a person rather than a sender of bulk mail, legal, financial, or irreplaceable. Anything shared with the wrong people.
- Finances: arithmetic, totals that do not reconcile, duplicates, miscategorized entries, anything that moves money or changes a bill.
- Reports: claims without evidence, conclusions the evidence does not support.
- Only comment on things that genuinely matter. If the work is clean, return an empty findings array.
- For each finding, set action to `ask-user` (a judgment only the boss can make), `auto-fix` (a clear mistake the minion can fix without asking), or `no-op` (informational).

Return JSON:

```json
{
  "findings": [
    { "severity": "error|warning|info", "action": "ask-user|auto-fix|no-op", "where": "item or line", "description": "..." }
  ],
  "risk_level": "low|medium|high",
  "risk_rationale": "one sentence"
}
```

### Code prompt

<Use this as the review agent's task. Fill the context fields.>

Review the code changes and return structured findings with a risk assessment.

Context:

- branch: <branch>
- base: <default branch or merge base>
- review scope: branch changes between base and the pushed tip
- ignore patterns: none, unless the project listed some

Task:

- Read the relevant history and diff yourself.
- Focus findings on risks introduced by changed code, but inspect surrounding code, call sites, shared helpers, tests, and invariants when needed to understand root cause.
- Do NOT run tests during review.
- Analyze for bugs, risks, and code simplification opportunities.
- Simplification means reducing code complexity through non-functional refactoring. It does NOT mean removing features or changing product behavior.
- Treat security issues, performance regressions, breaking changes, and insufficient error handling as risks.
- Complete the full review before returning. Do not stop after the first valid finding.

Rules:

- Anchor every finding to a specific file and one-indexed line number in the changed code when possible.
- Severity `error` must not merge. `warning` can be a follow-up. `info` is nice to have.
- Be concise and actionable. No generic advice like "add more tests".
- Only comment on things that genuinely matter.
- Do NOT report styling, formatting, linting, compilation, or type-checking issues.
- If the change is clean, return an empty findings array.
- For each finding, set action to one of:
  - `ask-user`: functional requirements, product behavior, or the author's deliberate intent. When in doubt, ask-user.
  - `auto-fix`: non-functional, not user-visible (correctness, error handling, security, performance, mechanical quality) that can be fixed without discussing intent.
  - `no-op`: informational.

Risk assessment after all findings:

- `low` if well-bounded and straightforward
- `medium` if room to improve but safe to raise first
- `high` if it should not raise without explicit human approval

Return JSON:

```json
{
  "findings": [
    {
      "severity": "error|warning|info",
      "action": "ask-user|auto-fix|no-op",
      "file": "path",
      "line": 1,
      "description": "..."
    }
  ],
  "risk_level": "low|medium|high",
  "risk_rationale": "one sentence"
}
```

## Loop

- `auto-fix`: fix the work (for code, send the findings to the coding agent). Then a new fresh review agent.
- `ask-user`: report it to Kevin with the work, who takes one decision card to the boss.
- `error`: do not present the work as ready.
- Empty findings, or only `info` / already-answered `ask-user`: the work is ready. Report it to Kevin; for code, the coding agent may open the pull request.

Fix-forward. Do not revert an intentional choice to silence a finding.

## Do not

- Do not let the review agent take the action under review
- Do not open a pull request to make a branch visible for review
