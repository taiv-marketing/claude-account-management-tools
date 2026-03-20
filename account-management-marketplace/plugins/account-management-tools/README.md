# Account Management Tools

Complete partnerships workflow for strategic partnerships managers. Automatically sync Gmail and Slack activity to a Notion partnerships database, backfill historical activity, and create new partner records with full context.

## What This Does

This plugin gives you six complementary skills that work together as a complete partnerships CRM:

- **account-sync-setup** - One-time initialization that verifies connectors, collects your Notion database ID, and validates your schema (run this first)
- **account-sync-24hr** - Daily sync that pulls the last 24 hours of Gmail and Slack activity and updates your Notion partner records
- **account-sync-30day** - One-time backfill that harvests 30 days of history and syncs it to Notion (run once after setup, then use the daily sync)
- **account-builder** - Research and create records for one account or a batch - just provide a name or a list
- **meeting-prep** - Pre-meeting brief for any partner - pulls their Notion page, recent emails, and Slack mentions into a fast-read summary
- **partner-digest** - Weekly view across all accounts - what moved, what went quiet, what needs attention

All skills use the same Notion database and maintain consistent formatting and matching logic.

## Prerequisites

You need these connectors installed and authenticated:

- **Gmail** - for email activity
- **Slack** - for message activity
- **Notion** - for your partnerships database

## Setup

### Quick Start

Just run the setup skill and it will walk you through everything:

```
Run account-sync-setup
```

Or ask naturally: "Set up account sync tools" or "Configure account sync"

The setup skill will:
1. Verify your Gmail, Slack, and Notion connectors are working
2. Ask for your Notion database ID
3. Validate your database has the right schema
4. Save the configuration so the other skills can use it

### Manual Setup (if you prefer)

If you want to set up manually instead of using the setup skill:

**1. Prepare Your Notion Database**

Create a Notion database (or use an existing one) with at least these fields:

- **Name** (Title) - Partner or company name
- **Domain** (Text or Email) - Company domain for matching Gmail activity
- **Status** (Select) - Deal stage or relationship status
- **Notes** (Text) - Activity log where synced updates will be appended
- **Last Contacted** (Date) - Optional but recommended

You can add other fields like contact person, deal size, owner, etc. The skills will work with whatever schema you have.

**2. Find Your Database ID**

1. Open your Notion partnerships database in your browser
2. Copy the URL - it looks like: `https://www.notion.so/workspace/abc123def456?v=...`
3. The database ID is the part between the last `/` and the `?` - in this example: `abc123def456`

The sync skills will ask for this ID the first time you run them.

## Usage

### Daily Sync (account-sync-24hr)

Run this daily (or set up a scheduled task to run it automatically):

```
Run account-sync-24hr
```

Or just ask naturally:
- "Sync my partnerships"
- "Update my Notion accounts"
- "Run my daily partner check"

The skill will:
1. Pull all Gmail threads from the last 24 hours
2. Pull all Slack messages from the last 24 hours
3. Match them to existing Notion records by domain or company name
4. Append concise notes to each matched record
5. Update the last contacted date

### One-Time Backfill (account-sync-30day)

Run this once when you first install the plugin to catch up on the last month of activity:

```
Run account-sync-30day
```

The skill splits the 30-day window into three 10-day blocks and processes them in reverse chronological order (most recent first). Each block completes fully before moving to the next, so if something stalls, you don't lose progress.

After running this once, use the daily sync going forward.

### Create New Partner Records (account-builder)

Whenever you meet a new partner or need to log a new account:

```
Create an account for Acme Corp
```

Or:
- "Add Stripe to Notion"
- "Log this new partner: Widget Co"
- "Build a record for Jane Smith at Acme"

The skill will:
1. Search Gmail and Slack for everything related to that company or person
2. Extract key details - domain, contacts, topics discussed, internal owner
3. Create a new Notion record with all the context

## How Matching Works

The skills use conservative matching to avoid false positives:

- **Gmail** - matches by sender domain (e.g., emails from `@acme.com` match a record with domain `acme.com`)
- **Slack** - matches by company name inference from message context
- **Skip if uncertain** - if a match isn't confident, the item is skipped rather than guessed

This means you might see "X items skipped" in the summary - that's normal and expected.

## Notes Format

All three skills append notes in this format:

```
[March 19] Renewal discussion via email - extending for 12 months. Slack: Jordan confirmed contract close to signing.
```

Notes are always appended, never overwritten. Each sync adds a new timestamped entry.

## Important Rules

- **Never creates duplicates** - The sync skills only update existing records, never create new ones. Use account-builder for new records.
- **Never overwrites notes** - All activity is appended to your existing notes field.
- **Never changes status without reason** - Status only changes if the activity clearly indicates a change.
- **Always asks for database ID** - The first time you run any skill, it will ask for your Notion database ID if you haven't provided it yet.

## Troubleshooting

**"Could not match X items"** - This is normal. The skills skip anything they can't confidently match to avoid creating noise.

**"Database ID not found"** - Provide your Notion database ID when prompted. The skills will remember it for future runs.

**"No activity found"** - If you have a quiet day or your partners haven't been active, the sync will report zero updates. That's fine.

## Advanced: Scheduled Daily Sync

You can set up the daily sync to run automatically every morning using the schedule skill:

```
Create a scheduled task to run account-sync-24hr every day at 9am
```

This way your Notion database stays updated without you having to remember to run the sync manually.
