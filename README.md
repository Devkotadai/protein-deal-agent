# Protein deal agent

A Claude agent that runs every morning, searches the web for protein deals, and emails you the best protein powders (top 10), bars (top 5) and drinks (top 5), ranked by price per 25 g of protein. It remembers what you've tried and whether you liked it, avoids ingredients you've flagged, watches what's running low, and logs every deal to Notion.

## What it does each morning

1. **Reads your Notion tracker:** liked and disliked products, ingredients to avoid, anything that needs restocking, and your Trusted Retailers list.
2. **Searches for deals** on powders, bars and drinks, only at retailers you've marked as trusted (no third-party marketplace sellers), and reads every price from the retailer's own product page that day. Prices it can't read (e.g. members-only Costco or Sam's Club) are listed as "couldn't verify" instead of guessed.
3. **Computes $ per 25 g protein and calories per gram of protein** for every candidate from the nutrition label, and checks the full ingredient list.
4. **Filters and ranks:** drops disliked products and avoided ingredients, then keeps the cheapest 10 powders, 5 bars and 5 drinks per 25 g of protein.
5. **Logs the deals** to the Daily Deals database and adds new products to Products.
6. **Emails the digest** so it lands by 9:00 AM Eastern.

It never buys anything. Every deal is a recommendation.

## The formula

```
$/g protein = price ÷ (grams protein per serving × servings per container)
```

The email leads with **$ per 25 g of protein** (about one scoop or shake), with $/g as a secondary number, plus **calories per gram of protein** and key ingredients with flags for every item. Details in [`docs/price-per-gram.md`](docs/price-per-gram.md).

## What's in this repo

| Path | What it is |
|---|---|
| [`prompts/daily-agent.md`](prompts/daily-agent.md) | The agent's daily instructions, with placeholders for your details |
| [`notion/schema.md`](notion/schema.md) | The five Notion databases: Products, Consumption Log, Restock Tracker, Daily Deals, Trusted Retailers |
| [`docs/price-per-gram.md`](docs/price-per-gram.md) | How price per gram of protein is calculated |
| [`docs/email-digest.md`](docs/email-digest.md) | The daily email's format |
| [`templates/email.html`](templates/email.html) | The HTML newsletter template the agent fills in ([preview](templates/email-preview.png)) |
| [`docs/setup.md`](docs/setup.md) | How to set it up and schedule it |

## Requirements

- Claude with scheduled tasks
- Connectors: Notion, Gmail, and optionally Shop
- Web search and fetch

## Privacy

No tokens, credentials, email addresses, or Notion workspace IDs are stored here. The prompt uses `{{placeholders}}`; keep your filled-in copy as a `*.local.md` file, which git ignores.
