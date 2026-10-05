---
name: Adversarial review
description: Use when Dr. Nefario has a pushed ship branch from the lab, before any pull request.
---

# Adversarial review

Review draft ship work on a pushed branch. Do not open a pull request until this review is clean.

## Who runs it

Dr. Nefario launches a **fresh** Cursor cloud agent in the lab for each review round, separate from the agent that wrote the code. Do not reuse the coding agent or an old review agent: the point is fresh eyes that did not write the change. Same settings as other lab agents (grok 4.6, high reasoning, not fast).

The review agent starts blank. The task must include the repo, branch, base, and this entire prompt. It reads the branch from the repo; it does not push.

## Prompt

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

- `auto-fix`: send the findings to the coding agent. Then a new fresh review agent.
- `ask-user`: report it to Kevin, who takes one decision card to the boss. Do not raise.
- `error`: do not raise.
- Empty findings, or only `info` / already-answered `ask-user`: the coding agent may open the pull request.

Fix-forward. Do not revert the author's intentional first commit to silence a finding.

## Do not

- Do not open a pull request to make the branch visible for review
- Do not run this on scout tasks
