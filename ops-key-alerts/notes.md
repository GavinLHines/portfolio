# Operations Key Alerts (Notes)

## What it does

Polls Slack's #operations-alert channel on a schedule, routes three types of alerts through parallel paths using Claude Haiku for reformatting and structured extraction, and delivers results to Google Chat for the operations team.

## Three alert paths

- Path A: Customer >30% inventory booked. Forwards the alert to Google Chat as-is.
- Path B: Hourly fulfillment summary. Claude Haiku reformats the warehouse performance summary, computing "Hrs to Complete" figures and handling missing-goal edge cases, then posts to Google Chat.
- Path C: VAS job error. Claude Haiku extracts 10 fields (fee quantity/unit/name, VAS job and request IDs, username, account and warehouse identifiers) and posts a formatted card to Google Chat with a WMS deep link.

## Migration context

Migrated from a 10-step Zapier workflow to n8n as the second workflow in CloudX's cost-reduction program. Took 3 builds to finish due to Slack API constraints:

1. Initial migration: 10 Zapier steps converted to n8n nodes.
2. Native Anthropic node refactor: replaced HTTP Request Claude calls with `@n8n/n8n-nodes-langchain.anthropic` nodes.
3. Polling redesign: supervisor ruled out a Slack bot, so the webhook trigger was replaced with a Schedule Trigger + GetChannelHistory poll and cross-execution timestamp deduplication to avoid processing messages twice.

## Technical highlights

- Per-item `$json` references used instead of pinned `$('Node').first()` because one poll can return multiple messages
- 4 Slack API blockers resolved: credential type mismatch, app provisioning, challenge verification failure, channel membership
- Network nodes have retry-on-fail, AI inputs have null guards, JSON parse has code-fence stripping, and missing extraction fields render blank to match the original Zapier behavior
- Runtime fix: Slack's `invalid_ts_oldest` error required switching from raw epoch to ISO date string in the oldest filter

## Scale

- Monitors an operations-alert channel, live in production
- Audience: operations leadership

## Proof (available on request)

The exported n8n workflow (Build 3, final) plus workflow canvas and execution-log screenshots are kept privately and available to prospective employers on request. The public version omits the workflow file because it contains embedded credentials.
