---
name: partner-digest
description: Weekly partnerships digest for strategic partnerships manager. Use this skill whenever the user wants a summary of recent activity across all partner accounts, or asks "what's happening with my partners", "give me a weekly digest", "what moved this week", "which accounts need attention", "partnerships summary", or any variation of wanting a high-level view across their book of accounts. Produces a readable summary - does not write to Notion. Run account-sync-setup first if this is your first time using the plugin.
example: "What's happening with my partners this week?"
---

# Partner Digest

A weekly read-only summary of what's happening across all partner accounts. Surfaces what moved, what went quiet, and what needs attention - without writing anything to Notion.

---

## Setup

This skill uses the same Notion database as the other account sync skills. The database ID should already be established. If not, ask the user for it before proceeding.

---

## Phase 1 - Set the Window

Default to the last 7 days. If the user specifies a different timeframe ("last two weeks", "this month"), use that instead.

---

## Phase 2 - Harvest Recent Activity

Pull from Gmail and Slack for the selected window - same bulk approach as the sync skills, not per-account searches.

### Gmail
- Search for all partner-related threads in the window
- For each relevant thread, capture: company domain, date, brief summary

### Slack
- Search all channels and DMs for partnership mentions in the window
- For each relevant message, capture: company name, channel or person, brief summary

Compile a raw activity list grouped by company before moving on.

---

## Phase 3 - Cross-Reference Notion

Fetch all records from the Notion database.

For each account:
- Check the last contacted date field if it exists
- Flag accounts with no activity in the harvest window as potentially stale
- Match harvest activity to records using the same domain and name inference logic as the sync skills

---

## Phase 4 - Build the Digest

Organize everything into a tiered summary. Keep each entry to one or two sentences.

Format:

```
Partner Digest - Week of March 13-19

Active This Week (3 accounts)

Acme Corp
Renewal discussion progressing - pricing call needed. Noah confirmed contract is close.

Stripe
New BD contact intro from Marcus Lee. Integration discussion starting.

Widget Co
Noah followed up on integration timeline via DM.

---

Quiet This Week (accounts with no recent activity)

Old Partner Inc - last activity March 1
Startup ABC - last activity February 22

---

Needs Attention

- Acme Corp: Pricing proposal still not sent - mentioned in two threads
- Old Partner Inc: No contact in 18 days - may be worth a check-in
```

Adjust the "needs attention" section based on what actually stands out - open action items mentioned in emails, accounts that have gone cold, or anything that looks like it's at risk of slipping.

---

## Rules

- Do not write anything to Notion - this is purely for reading
- Do not fabricate activity - if an account had no activity in the window, list it as quiet
- If no activity is found at all, say so rather than producing an empty digest
- Keep the digest concise - the goal is a fast scan, not a full report
- If the database ID has not been provided, stop and ask before doing anything else
