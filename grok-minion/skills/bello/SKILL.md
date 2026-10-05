---
name: Bello
description: Use when the boss explicitly says bello or /bello (or ahoy), or asks for a session recap of what happened since they last spoke, plus any visibly unanswered decisions. A standalone boss message whose main ask is "bello" is an invocation. History-only. Do not gather live state.
---

# Bello

Give a concise session-only recap. Do not gather fresh state.

Treat an explicit "bello" (or the old "ahoy") the same as `/bello`. Slash is optional. Do not wait for a slash-command invocation.

## What counts as a boss message

A boss boundary is an ordinary user message they typed.

Exclude:

- Minion or other bot messages (`[agent]`, a cloud agent or bot reporting in)
- Scheduled or event wakes (`[routine]`)
- System, tool, and other injected operational messages
- The current bello invocation itself, with or without a slash

A previous bello is a real boss message and may be the next interval boundary.

## Recap

1. Find the most recent real boss-authored message before this invocation.
2. If none exists, say this session has no prior boss message and stop. Do not invent a live status snapshot. Do not call GitHub or read live queues.
3. Recap only what is already visible after that message and before this invocation.
   Include concrete outcomes, landed work, failures, decisions made, new decisions needed, and work still running only when those events appear in that interval.
   Use outcome language. Keep every full PR URL that appears in the interval.
4. Also inspect the entire visible session before this invocation for every explicit boss decision that is still unanswered.
   A later unrelated boss message is a recap boundary. It does not close an earlier decision.
   A decision is closed only when a later visible response substantively resolves it: they chose an option, declined it, granted or denied the approval, skipped a card (treat skip as decline and record the assumption you made), or otherwise directly addressed it.
   Deduplicate by substance.
5. Do not call GitHub, browsers, live status snapshots, or file writes. Create no report. Do not guess live state beyond the last visible event.
6. If nothing happened after the previous boss message but an older open decision is still visible, report that decision instead of claiming nothing happened.
7. If neither events nor open decisions exist, say in one sentence that nothing happened after the previous boss message.

## Decisions

After the recap, if any visibly open decisions remain, present only the single most impactful one.

Cover: what it is, why a decision is needed now, the real options, and a recommendation with a one-line why. Put the options on a choice card. One card at a time.

When they answer, present the next highest-impact remaining decision the same way, until none remain.

Do not start this flow when the inventory is empty.
Do not batch unrelated decisions onto one card.

## Do not

- Do not treat a good leftover merge as an open decision just because the process was messy
- Do not re-ask a skipped card; the skip already closed it
- Do not pull live issue or PR status to "complete" the recap
