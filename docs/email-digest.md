# Email digest format

One HTML email per day, sent so it lands by 9:00 AM Eastern. The agent fills in [`templates/email.html`](../templates/email.html); the notes at the top of that file say how.

**Subject:** `Top protein deals: <Mon Day> · powder $X.XX, bar $X.XX, drink $X.XX per 25 g`

## Layout, top to bottom

1. **Header:** date and "Today's 10 best buys".
2. **Stats strip:** best $ per 25 g for powders, bars and drinks.
3. **Running low** (only when something needs restocking): what's low, days left, and which deal covers it.
4. **Top powder pick card:** product, retailer and seller, a big **$ per 25 g** figure, price (with the old price struck through only if the page shows it), total protein, $/g, discount, code, expiry, tag, key ingredients, and a "View deal" button.
5. **Powders #2 to #10, then Protein bars (top 5), then Protein drinks (top 5):** one row each with rank, product link, retailer, price, total protein, $/g, **calories per gram of protein**, a key-ingredients line with flags, tags, and a color-coded **$ per 25 g** chip on the right (green up to $0.80, amber up to $1.40, red above).
6. **Couldn't verify** (only when needed): Costco or Sam's Club products whose prices weren't readable online today, with no price shown.
7. **Footer:** a nudge to rate products in Notion, the time prices were checked, what was skipped and why, and "Nothing was purchased."

$ per 100 g is not shown. Calories per gram of protein = calories ÷ protein per serving (about 4 is near-pure protein; green up to 5.0, amber up to 8.0, red above).

If fewer than 10 deals could be verified, the email says so in the stats strip and footer. If the search failed entirely, a short plain email explains what failed.

## Readability rules (Nov 3 redesign)

- Powders, bars and drinks each get their own card with a colored header band (green, orange, blue) and a gap between cards.
- Text is dark on white; white text appears only inside dark colored bands.
- Gmail drops the CSS `background:` shorthand. Use the `bgcolor` attribute plus `background-color` on every colored cell.
- Sizes: product names 17px, details 15px, ingredients 14px, nothing under 13px.
