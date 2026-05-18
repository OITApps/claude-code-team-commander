# Persona Routing Table

Personas are behavior profiles for AI personalities. Only Jarvis remains active for Claude Code work; Cloudie's behavior lives in n8n. [Your Company] domain work is now skill-based (see Phase I — specialist skill routing).

## Active Personas

| Persona | Scope | Profile |
|---------|-------|---------|
| Jarvis Merriweather | Personal executive assistant (Ray only) | Lives in [`[YourGitHubOrg]/automations`](https://github.com/[YourGitHubOrg]/automations/blob/main/) — see `automations/.claude/personas/` (this file is outside claude-config scope) |

## Deprecated 2026-05-18 (Phase I.0 — specialist skill routing)

The following Claude Code personas were retired in favor of skill-based dispatch:

- **[Persona 1 - e.g. "Flo Rivers"]** — replaced by `flow-review`, `sf-smoke-as`, and other Salesforce skills
- **[Persona 5 - e.g. "Holly Helpdesk"]** — Support Request workflow folded into commands that load `runbooks/salesforce-cli.md`
- **[Persona 3 - e.g. "Paige Turner"]** — replaced by `docs-update`, `voip-research` skills
- **[Persona 2 - e.g. "Stan Dardson"]** — case review workflow stays in `/stan-review`, `/stan-patrol`, etc. (commands keep the voice; persona file removed)
- **[Persona 4 - e.g. "Stella Fullstack"]** — replaced by language/runtime skills (`wordpress-pro`, generic dev with on-demand MCP loads)

Their n8n consolidation into Cloudie happened 2026-04-28; the Claude Code surface follows here.

## Teams Bot Integration

When a Cloudie/Jarvis interaction needs to post to a Teams thread or channel, use the Bot Framework Connector API:

1. Get an OAuth token: `POST https://login.microsoftonline.com/{TENANT_ID}/oauth2/v2.0/token` with `client_credentials` grant, `scope=https://api.botframework.com/.default`, using the bot's `CLIENT_ID` and `CLIENT_SECRET` env vars
2. Send message: `POST {serviceUrl}/v3/conversations/{conversationId}/activities` with the bot token
3. The message appears as the bot identity (e.g. "Cloudie McCloudie"), not the admin M365 account

### Bot Registry

| Bot | Client ID Env Var |
|-----|-------------------|
| Cloudie McCloudie | `CLOUDIE_MCCLOUDIE_CLIENT_ID` |
| Jarvis Merriweather | `JARVIS_MERRIWEATHER_CLIENT_ID` |

Retired bot client IDs (Flo, Holly, Stan, Paige, Stella) are still present in Azure but no longer dispatched.

### Service URL Discovery

For replies to threads already fetched via m365 MCP, the service URL is `https://smba.trafficmanager.net/amer/`. The `conversationId` for a channel is the channel ID (e.g. `19:xxx@thread.skype`).
