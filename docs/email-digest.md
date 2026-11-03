# Email digest format

One email per day, sent so it lands by 9:00 AM Eastern.

**Subject:** `Top 10 protein deals: <Month Day>` (e.g. `Top 10 protein deals: Nov 4`)

**Body** (HTML if supported, otherwise clean plain text):

1. **Summary line:** the best $/g today and anything that needs restocking.
2. **Numbered table of up to 10 deals**, sorted by $/g ascending:

   | # | Product & flavor | Retailer | Price | Total protein (g) | $/g protein | $ per 25 g | $ per 100 g | Discount / code / expiry | Tag | Link |
   |---|---|---|---|---|---|---|---|---|---|---|

   Tags: `Restock needed`, `Liked before`, `New to try`, `Ingredient flag`.
3. **Under each deal:** one short line with key ingredients or avoid-flags.
4. **Footer:** a link to the Notion home page and a reminder that ratings in the Consumption Log improve future picks.

If fewer than 10 deals could be verified, send what was found and say so. If the search failed entirely, send a short email explaining what failed.

## Example (plain text)

```
Top 10 protein deals: Nov 4

Best today: $0.028/g ($0.70 per 25 g). Nothing needs restocking.

1. Example Brand Whey, Double Chocolate (5 lb) at Example Store
   $49.99 · 1,776 g protein · $0.028/g · $0.70/25 g · $2.81/100 g
   20% off, code SAVE20, ends Nov 8 · New to try · https://example.com/...
   Whey concentrate, whey isolate, cocoa, sucralose (flag: Sucralose)
...
Your tracker: <Notion home page link>
Rate what you try in the Consumption Log and tomorrow's picks get better.
```
