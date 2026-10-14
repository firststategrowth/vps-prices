# VPS prices, read daily

Every row is one hosting plan (VPS and cloud servers, plus a few managed plans), priced in US dollars
a month, read from the provider's own pricing page or price API on the date in `checked_at`, with the
link in `source_url`. `price_source` says whether a row was read from the page, from the provider's
price API, or by hand; rows read by hand can be a few days old.

On 14 Oct 2026 the file has 231 plans from 49 providers. Plans the provider shows as sold out
on that day are left out.

Three things that change what you pay are in their own columns:
- `ipv4_extra_usd`: what a public IPv4 address adds to `monthly_usd`. Scaleway ($4.08/mo), phoenixNAP ($5.11/mo) and Mythic Beasts ($2.65/mo) bill it on top. Hetzner and netcup print their prices with the address included; for them `monthly_usd` and, where present, `billed_eur` are the price without it, and `monthly_usd` + `ipv4_extra_usd` is what they print.
  Empty when the provider's page shows no separate charge.
- `excludes_vat`: true when the provider's page states the price before VAT
- `commitment_months`: above 1, the price needs a prepaid term (Hostinger KVM 1 is $6.49/mo paid
  24 months up front; it renews at a higher price, and renewal prices are not in this file)

Providers that print euro prices keep the euro figure in `billed_eur`; it is converted at the latest
European Central Bank reference rate (13 Oct 2026: 1 EUR = $1.1188; also in `fx` in prices.json). Webdock and Exoscale bill the same figures in US dollars, so a dollar account there pays less than `monthly_usd`.
Other providers that publish dollar prices are taken in dollars.

Kinsta's managed WordPress plans are sold by monthly visits (`monthly_visits`, `site_limit`), not by size, so their `vcpu` and `ram_gb` are empty.

## Files
- `prices.csv` and `prices.json`: today's plans
- `changes.json`: dated list-price changes, each with its source (4 so far; a move in the exchange
  rate is not a change)
- `year-on-year.csv`: each provider's pricing page as the Wayback Machine archived it (19 Sep 2025 to 13 Dec 2025, most in October 2025;
  date per row in `archived_on`) against today's price, plan by plan, in the currency the provider prints
  (`currency`); `comparable` is "no" where the spec or terms changed. It keeps plans that are sold out today.

## Use it
CC BY 4.0. Credit "StackRates (stackrates.ai)" with a link to https://stackrates.ai/data/.

Ask it in plain English: https://stackrates.ai/ask/
No-key API: `https://stackrates.ai/api/ask?q=cheapest+4+GB+VPS+in+Germany`
For AI assistants (MCP, Streamable HTTP): `https://stackrates.ai/mcp`

## Who runs this
StackRates, First State Growth LLC. We may earn a commission or referral reward when someone signs up
through our links (from some providers it is account credit, not cash); the site names those providers
on every page. It never changes a price we list or the order of a list. Corrections: https://stackrates.ai/about/
