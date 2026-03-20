---
name: meeting-prep
description: Pre-meeting briefing skill for strategic partnerships manager. Use this skill whenever the user has an upcoming call or meeting with a partner and wants to prepare. Triggers on phrases like "prep me for my call with", "I have a meeting with", "briefing on", "what do I know about", or "catch me up on". Pulls the partner's Notion page, recent emails, and Slack mentions into a concise brief. Run account-sync-setup first if this is your first time using the plugin.
example: "Prep me for my Acme call"
---

# Meeting Prep

Pull everything known about a partner into a concise pre-meeting brief - their Notion page, recent email threads, and Slack mentions - so you walk in with full context.

---

## Setup

This skill uses the same Notion database as the other account sync skills. The database ID should already be established. If not, ask the user for it before proceeding.

---

## Phase 1 - Identify the Account

The user will name a company or contact. If they haven't, ask who the meeting is with.

Search the Notion database for the matching record. If multiple records match, show the options and ask the user to confirm which one.

---

## Phase 2 - Pull All Context

Gather from three sources in parallel:

### Notion
- Fetch the full page content for the matched record
- Note all property fields (status, domain, contact, last contacted, etc.)
- Read the full page body - this is the activity history

### Gmail
- Search for all threads with this company's domain or contact name from the last 30 days
- Capture: dates, subjects, key points discussed, any open items or commitments made

### Slack
- Search for mentions of this company across all channels and DMs from the last 30 days
- Capture: who internally has been discussing this account, what was said, any decisions or context

---

## Phase 3 - Build the Brief

Synthesize everything into a single structured brief. Keep it tight - this should be readable in under two minutes.

Format:

```
Meeting Brief - Acme Corp
[Date] | [Contact name and title if known]

Relationship Overview
One or two sentences on where things stand - stage, history, general vibe.

Recent Activity
- March 15 (Email): Renewal discussion - they want to extend for 12 months, pricing TBD
- March 12 (Slack): Noah flagged contract is close to signing
- March 5 (Email): Intro to new procurement contact, Jane Smith

Open Items
- Pricing proposal not sent yet
- Follow-up call was scheduled but no confirmation received

Key Contacts
- Jane Smith - jane@acme.com - procurement lead
- Tom Barnes - tom@acme.com - original contact

Suggested Talking Points
- Confirm timeline on renewal
- Introduce new pricing structure
- Clarify who signs off on their end
```

Keep the suggested talking points to three or four - specific and actionable, not generic.

---

## Rules

- If no Notion record is found for this company, say so and offer to create one with account-builder
- If no recent Gmail or Slack activity is found, note that in the brief rather than fabricating context
- Do not write anything back to Notion - this is read-only
- If the database ID has not been provided, stop and ask before doing anything else
