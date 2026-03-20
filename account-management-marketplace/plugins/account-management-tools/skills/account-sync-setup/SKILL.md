---
name: account-sync-setup
description: Initialize and configure the account management tools plugin. Use this skill when the user asks to set up account sync, configure account management tools, initialize the plugin, check account sync setup, validate connectors, or mentions this is their first time using the plugin. Also use when troubleshooting connection issues or when the user wants to verify their Notion database is configured correctly. Start here before using any other skill in this plugin.
example: "Set up my account sync"
---

# Account Sync Setup

One-time configuration for the account sync tools plugin. Validates connectors, collects your Notion database ID, confirms schema requirements, and ensures everything is ready before you run your first sync.

---

## Phase 1 - Verify Connectors

Check that all three required connectors are installed and authenticated:

1. **Gmail** - Try listing labels or fetching profile to confirm access
2. **Slack** - Try searching channels or listing users to confirm access
3. **Notion** - Try searching or listing a page to confirm access

For each connector:
- If working: ✓ mark as ready
- If missing or not authenticated: ✗ explain what needs to be done and provide instructions

Do not proceed to Phase 2 until all three connectors are working.

Output a connector status table:

```
Connector Status:
- Gmail: ✓ Connected (dylan@taiv.tv)
- Slack: ✓ Connected (Taiv workspace)
- Notion: ✓ Connected
```

---

## Phase 2 - Collect Database ID

Ask the user for their Notion partnerships database ID.

Provide clear instructions:

```
I need your Notion partnerships database ID to configure the sync.

To find it:
1. Open your partnerships database in Notion
2. Copy the URL from your browser
3. The database ID is the part between the last / and the ?

Example: https://notion.so/workspace/abc123def456?v=xyz
Database ID: abc123def456

What's your database ID?
```

Once provided, store it in your working context with this exact format:

```
ACCOUNT SYNC CONFIG
Notion Database ID: [the ID they provided]
```

This format allows the other skills (account-sync-24hr, account-sync-30day, account-builder) to reference it automatically.

---

## Phase 3 - Validate Database Schema

Fetch the Notion database using the provided ID and inspect its schema.

Check for these recommended fields:
- **Name or Title field** (required) - for partner/company name
- **Domain or Email field** (recommended) - for matching Gmail activity
- **Status field** (recommended) - for tracking deal stage
- **Last Contacted field** (optional) - will be updated by syncs if present

Note: Activity updates and notes are written directly to each record's page body content, not to a property field. No notes field is needed.

Output a schema validation table:

```
Database Schema:
✓ Name (Title)
✓ Domain (Text)
✓ Status (Select: Prospect, Active, Closed)
✓ Last Contacted (Date)
✓ Contact Person (Text)

All required fields present. Activity will be written to page body. Ready for sync.
```

If any required fields are missing, explain what needs to be added and why it's needed.

If the database structure looks unusual (no obvious name field, no domain field), flag it and ask the user to confirm this is the right database.

---

## Phase 4 - Setup Summary

Provide a complete setup summary and next steps:

```
Setup Complete

✓ Connectors verified (Gmail, Slack, Notion)
✓ Database ID saved: abc123def456
✓ Schema validated: 6 fields detected

Your account sync tools are ready. Here's what you can do now:

**One-time backfill** (recommended first step)
Run account-sync-30day to pull 30 days of history into Notion

**Daily sync** (set and forget)
Run account-sync-24hr daily, or schedule it to run automatically every morning

**Create new records**
Use account-builder whenever you need to log a new partner

All three skills will use the database ID I just saved, so you won't need to enter it again.
```

---

## Rules

- Do not proceed past Phase 1 until all connectors are working
- Do not proceed past Phase 2 until you have a valid database ID
- Store the database ID in the exact format specified so other skills can find it
- If the user already has the database ID saved from a previous run, skip Phase 2 and just validate it
- If schema validation reveals missing critical fields, provide specific instructions on what to add to Notion
