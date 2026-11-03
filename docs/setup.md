# Setup

## 1. Build the Notion workspace

Create a page (e.g. "Protein Deal Agent") and add the four databases described in [`notion/schema.md`](../notion/schema.md), with the relations and formulas shown there. Copy each database's data source URL (`collection://...`) and the page URL.

## 2. Fill in the prompt

Open [`prompts/daily-agent.md`](../prompts/daily-agent.md) and replace:

| Placeholder | Value |
|---|---|
| `{{YOUR_NAME}}` | Your name |
| `{{YOUR_EMAIL}}` | Where the digest goes |
| `{{PRODUCTS_DATA_SOURCE_URL}}` etc. | The four `collection://...` URLs |
| `{{NOTION_HOME_PAGE_URL}}` | The Notion page URL |

Keep the filled-in copy out of the repo (see `.gitignore`), since it contains your email address and workspace IDs.

## 3. Connect the tools

The agent needs these connectors in Claude:

- **Notion**: read preferences and restock needs, log deals
- **Gmail**: send the digest
- **Shop** (optional): search product catalogs
- Web search and web fetch: find and verify live prices

## 4. Schedule it

Create a daily scheduled task in Claude with the filled-in prompt. The original runs at **8:34 AM America/New_York** so the email lands before 9:00:

```
CRON_TZ=America/New_York 34 8 * * *
```

## 5. Teach it your taste

Rate what you try in the Consumption Log, set Verdicts and Avoid flags in Products, and keep the Restock Tracker current. The agent reads all of it each morning.
