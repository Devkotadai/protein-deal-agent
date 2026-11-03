# Notion database layout

Everything lives under one Notion page (e.g. "Protein Deal Agent") with four databases. Relations link them together so the agent can see what you liked, what's running low, and what it found before.

```
Products ──< Consumption Log
   │
   ├──< Restock Tracker
   │
   └──< Daily Deals
```

## Products

Every protein product bought or considered.

| Property | Type | Notes |
|---|---|---|
| Name | Title | |
| Brand | Text | |
| Type | Select | Whey, Whey isolate, Casein, Plant, Bar, RTD shake, Jerky/meat, Other |
| Flavor | Text | |
| Protein per serving (g) | Number | From the nutrition label |
| Servings per container | Number | From the nutrition label |
| Total protein (g) | Formula | `Protein per serving (g) × Servings per container` |
| Ingredients | Text | Full ingredient list from the label |
| Avoid flags | Multi-select | Sucralose, Artificial colors, Seed oils, Sugar alcohols, Soy, Gluten, Dairy |
| Verdict | Select | Love, OK, Disliked, Untried |
| Best price seen ($) | Number (dollar) | Updated by the agent |
| Best $/g protein | Number (dollar) | Updated by the agent |
| Product URL | URL | |
| Notes | Text | |
| Consumption | Relation → Consumption Log | |
| Restock | Relation → Restock Tracker | |
| Deals | Relation → Daily Deals | |

## Consumption Log

What you ate or drank and whether you liked it. Feeds the agent's taste preferences.

| Property | Type | Notes |
|---|---|---|
| Entry | Title | |
| Date | Date | |
| Product | Relation → Products | |
| Servings | Number | |
| Liked? | Select | Liked, Meh, Disliked |
| Rating (1-5) | Number | |
| Why | Text | |

## Restock Tracker

What's on hand and when it runs out.

| Property | Type | Notes |
|---|---|---|
| Item | Title | |
| Product | Relation → Products | |
| Servings left | Number | |
| Servings per day | Number | |
| Days left | Formula | `Servings left ÷ Servings per day` |
| Reorder when days left ≤ | Number | Your reorder threshold |
| Needs restock | Formula | `Days left ≤ Reorder when days left ≤` |
| Target $/g protein | Number (dollar) | Price you'd be happy to pay |
| Last updated | Date | |

## Daily Deals

What the agent found each morning.

| Property | Type | Notes |
|---|---|---|
| Deal | Title | |
| Date found | Date | |
| Product | Relation → Products | |
| Retailer | Text | |
| Price ($) | Number (dollar) | Final price after discounts |
| Total protein (g) | Number | Protein per serving × servings × pack count |
| $/g protein | Formula | `Price ($) ÷ Total protein (g)` |
| $ per 25g protein | Formula | `Price ($) ÷ Total protein (g) × 25` |
| Discount | Text | |
| Promo code | Text | |
| Expires | Date | |
| Link | URL | |
| Fit | Select | Liked before, New to try, Ingredient flag, Disliked before |
| Status | Select | New, Bought, Skipped |

### Notion formula syntax

```
// Products › Total protein (g)
prop("Protein per serving (g)") * prop("Servings per container")

// Restock Tracker › Days left
if(prop("Servings per day") > 0, round(prop("Servings left") / prop("Servings per day")), empty())

// Restock Tracker › Needs restock
prop("Days left") <= prop("Reorder when days left ≤")

// Daily Deals › $/g protein
if(prop("Total protein (g)") > 0, round(prop("Price ($)") / prop("Total protein (g)") * 1000) / 1000, empty())

// Daily Deals › $ per 25g protein
if(prop("Total protein (g)") > 0, round(prop("Price ($)") / prop("Total protein (g)") * 25 * 100) / 100, empty())
```

## Trusted Retailers

The only stores the agent may use. Edit freely: untick **Active** to pause a store, add a row to allow a new one.

| Property | Type | Notes |
|---|---|---|
| Retailer | Title | Store name |
| Website | URL | Home page |
| Active | Checkbox | Only checked rows are used |
| Seller rule | Text | Which sellers count, e.g. "Only Sold and shipped by Walmart.com" |
| Type | Select | Big-box, Warehouse club, Supplement store, Brand site, Grocery |
| Notes | Text | Extra handling, e.g. "If the price isn't online, list under Couldn't verify" |

## Columns added for bars and drinks

Both **Products** and **Daily Deals** have:

| Property | Type | Notes |
|---|---|---|
| Category | Select | Powder, Bar, Drink |
| Calories per serving | Number | From the Nutrition Facts panel |
| Cal per g protein | Formula | `Calories per serving ÷ Protein per serving (g)`, 1 decimal |

**Daily Deals** also has **Protein per serving (g)** (number), **Key ingredients / flags** (text), and **$ per 25g protein** (formula: `Price ÷ Total protein × 25`).
