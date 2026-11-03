# Protein deal agent

A Claude agent that runs every morning, searches the web for protein deals, and emails you the top 10 ranked by **price per gram of protein**, never per ounce.

It remembers what you've tried and whether you liked it, avoids ingredients you've flagged, watches what's running low, and logs every deal to Notion.

## What it does each morning

1. **Reads your Notion tracker:** liked and disliked products, ingredients to avoid, and anything that needs restocking.
2. **Searches for deals** across major US retailers and brand sites, and verifies each price on a live product page.
3. **Computes price per gram of protein** for every candidate from the nutrition label.
4. **Filters and ranks:** drops disliked products and avoided ingredients, then keeps the 10 cheapest per gram.
5. **Logs the deals** to the Daily Deals database and adds new products to Products.
6. **Emails the digest** so it lands by 9:00 AM Eastern.

It never buys anything. Every deal is a recommendation.

## The formula

```
$/g protein = price ÷ (grams protein per serving × servings per container)
```

The digest also shows **$ per 25 g** (about one scoop) and **$ per 100 g**. Details in [`docs/price-per-gram.md`](docs/price-per-gram.md).

## What's in this repo

| Path | What it is |
|---|---|
| [`prompts/daily-agent.md`](prompts/daily-agent.md) | The agent's daily instructions, with placeholders for your details |
| [`notion/schema.md`](notion/schema.md) | The four Notion databases: Products, Consumption Log, Restock Tracker, Daily Deals |
| [`docs/price-per-gram.md`](docs/price-per-gram.md) | How price per gram of protein is calculated |
| [`docs/email-digest.md`](docs/email-digest.md) | The daily email's format |
| [`docs/setup.md`](docs/setup.md) | How to set it up and schedule it |

## Requirements

- Claude with scheduled tasks
- Connectors: Notion, Gmail, and optionally Shop
- Web search and fetch

## Privacy

No tokens, credentials, email addresses, or Notion workspace IDs are stored here. The prompt uses `{{placeholders}}`; keep your filled-in copy as a `*.local.md` file, which git ignores.
