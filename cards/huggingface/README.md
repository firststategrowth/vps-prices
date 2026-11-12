---
license: cc-by-4.0
pretty_name: StackRates VPS prices and monthly price index
language:
- en
tags:
- vps
- cloud-computing
- web-hosting
- prices
- price-index
- time-series
size_categories:
- 10K<n<100K
configs:
- config_name: prices
  data_files: "prices.csv"
  skiprows: 1
  default: true
- config_name: price_history
  data_files:
  - "history/20i.csv"
  - "history/accuwebhosting.csv"
  - "history/advinservers.csv"
  - "history/akamai-linode.csv"
  - "history/alphavps.csv"
  - "history/alwyzon.csv"
  - "history/arubacloud.csv"
  - "history/atlanticnet.csv"
  - "history/aws-lightsail.csv"
  - "history/bandwagonhost.csv"
  - "history/cherryservers.csv"
  - "history/civo.csv"
  - "history/cloudcone.csv"
  - "history/cloudserver.csv"
  - "history/cloudways.csv"
  - "history/contabo.csv"
  - "history/crowncloud.csv"
  - "history/databasemart.csv"
  - "history/dedirock.csv"
  - "history/digitalocean.csv"
  - "history/dreamhost.csv"
  - "history/elementor.csv"
  - "history/ethernetservers.csv"
  - "history/evolutionhost.csv"
  - "history/evoxt.csv"
  - "history/exoscale.csv"
  - "history/extravm.csv"
  - "history/fdcservers.csv"
  - "history/glesys.csv"
  - "history/greencloudvps.csv"
  - "history/heartinternet.csv"
  - "history/hetzner.csv"
  - "history/hivelocity.csv"
  - "history/hostcolor.csv"
  - "history/hosthatch.csv"
  - "history/hostingcom.csv"
  - "history/hostingde.csv"
  - "history/hostinger.csv"
  - "history/hostnamaste.csv"
  - "history/hostnoc.csv"
  - "history/hostodo.csv"
  - "history/hostpapa.csv"
  - "history/hostus.csv"
  - "history/hostwinds.csv"
  - "history/hyonix.csv"
  - "history/iniz.csv"
  - "history/interserver.csv"
  - "history/ionos.csv"
  - "history/kamatera.csv"
  - "history/khanwebhost.csv"
  - "history/kimsufi.csv"
  - "history/kinsta.csv"
  - "history/knownhost.csv"
  - "history/krystal.csv"
  - "history/latitudesh.csv"
  - "history/liquidweb.csv"
  - "history/liteserver.csv"
  - "history/lunanode.csv"
  - "history/luxvps.csv"
  - "history/mythicbeasts.csv"
  - "history/namescouk.csv"
  - "history/netart.csv"
  - "history/netcup.csv"
  - "history/nocix.csv"
  - "history/ovhcloud.csv"
  - "history/pawnhoster.csv"
  - "history/phoenixnap.csv"
  - "history/prohosting24.csv"
  - "history/quickpacket.csv"
  - "history/racknerd.csv"
  - "history/raiola.csv"
  - "history/ramnode.csv"
  - "history/reliablesite.csv"
  - "history/scaleway.csv"
  - "history/securedragon.csv"
  - "history/servarica.csv"
  - "history/serverhost.csv"
  - "history/smokyhosts.csv"
  - "history/softshellweb.csv"
  - "history/spinservers.csv"
  - "history/strato.csv"
  - "history/swhosting.csv"
  - "history/tiernet.csv"
  - "history/time4vps.csv"
  - "history/ukhost4u.csv"
  - "history/ultahost.csv"
  - "history/ultravps.csv"
  - "history/ultrawebhosting.csv"
  - "history/upcloud.csv"
  - "history/veesp.csv"
  - "history/vexxhost.csv"
  - "history/virtono.csv"
  - "history/vpsbg.csv"
  - "history/vpsdime.csv"
  - "history/vultr.csv"
  - "history/webdock.csv"
  - "history/webhosting24.csv"
  - "history/webhostinguk.csv"
  skiprows: 1
- config_name: price_index
  data_files: "index/vps-price-index.csv"
  skiprows: 1
- config_name: price_index_plans
  data_files: "index/vps-price-index-plans.csv"
  skiprows: 1
- config_name: year_on_year
  data_files: "year-on-year.csv"
  skiprows: 1
---

# StackRates VPS prices and monthly price index

StackRates reads VPS and cloud server prices from each provider's own pricing page or price API every day and
publishes every read. On 12 November 2026 the daily file has 702 orderable plans from 97 providers; the
history holds 10,257 reads of 98 providers' plans over 38 read days, 5 October 2026 to 11 November 2026.

The newest index figure, as the site cites it: StackRates VPS price index, Nov 2026: 4 GB median $14.02/mo over 93 plans with 4 GB (506 plans in the index), read 11 Nov 2026, stackrates.ai/vps-price-index/

## Files

| Subset | File | Rows | What |
|---|---|---:|---|
| `prices` | `prices.csv` | 702 | today's orderable plans, read 12 November 2026 |
| `price_history` | `history/<provider>.csv` | 10,257 | every price read, 5 October 2026 to 11 November 2026, one file per provider |
| `price_index` | `index/vps-price-index.csv` | 2 | the monthly VPS price index: medians at 1, 4 and 8 GB of RAM |
| `price_index_plans` | `index/vps-price-index-plans.csv` | 799 | every plan behind each month of the index |
| `year_on_year` | `year-on-year.csv` | 128 | each plan against its October 2025 Wayback Machine capture |

Every CSV opens with one line, `# Source: StackRates, https://stackrates.ai/data/, CC BY 4.0, read <date>`, before
its header. Skip it: `pandas.read_csv(url, skiprows=1)`.

## Provenance
- Each daily row was read on `checked_at` from `source_url`, by the provider's public API, by our price run on its
  pricing page, or by a team member (`price_source`); a history row says the same in `read_date` and `read_method`.
- Prices are regular list prices as the provider prints them: introductory and sale prices are not used; most EU
  providers print before VAT, US providers before sales tax. A public IPv4 address billed on top is its own column.
- Euro and pound prices without the provider's own dollar price are converted at the European Central Bank reference
  rate: the read day's in the history, the latest in `prices.csv` (given in `fx` in `prices.json`).
- 86 history rows carry a figure corrected after the read; `history/revisions.csv` lists each with the figure as read.
- Nothing before 5 October 2026 counts as a read. The year-on file compares Wayback Machine captures of the providers'
  pages from around October 2025, one capture per row (`archived_page`, `archived_on`).
- The index method (which plans count, which price, the weighting) is in `index/vps-price-index.json` and at
  https://stackrates.ai/vps-price-index/

## Limits
- 97 providers in today's file (98 in the history), not the whole market: the ones StackRates
  reads, listed in `history/README.md`.
- The index's newest month runs to its last read day (`last_read`); its medians move when plans enter or leave, so
  read `change_pct_vs_prev_month` (the same plans in both months) for price moves.
- List prices, not what a customer paid after tax, coupons or renewal.

## Schemas
### `prices.csv`

| Column | Type | Meaning |
|---|---|---|
| `id` | string | Stable plan identifier. |
| `provider` | string | Provider slug. |
| `provider_name` | string | The provider's name. |
| `name` | string | The provider's plan name. |
| `monthly_usd` | numeric | List price in US dollars a month. Providers that price in euros: the provider's own dollar table where one exists, else the euro price at the ECB reference rate (fx in prices.json). |
| `billed_eur` | numeric | The euro list price (without VAT) for a provider that bills only in euros. |
| `billed_gbp` | numeric | The pound list price for a provider that bills only in pounds. |
| `vcpu` | numeric | Virtual CPUs as listed; empty when not stated (managed WordPress plans sold by visits or sites). |
| `ram_gb` | numeric | RAM in GB as listed; empty when not stated. |
| `storage_gb` | numeric | Disk in GB included in the price. |
| `storage_type` | string | Disk type as listed (NVMe, SSD, HDD). |
| `transfer_tb` | numeric | Monthly traffic allowance in TB; empty when unlimited under a fair-use policy or not stated. |
| `monthly_visits` | numeric | Managed WordPress plans: monthly visits included; empty where the plan names none. |
| `site_limit` | numeric | Managed WordPress plans: sites included. |
| `cpu_type` | string | CPU type as listed (shared, dedicated, the processor family where stated). |
| `excludes_vat` | boolean | true when the provider's page states the price before VAT. |
| `ipv4_extra_usd` | numeric | What a public IPv4 address adds to monthly_usd, when billed on top; empty when the page shows no separate charge. |
| `ipv6_only` | boolean | true when the plan has no public IPv4 address. |
| `ipv4_not_stated_read_at` | datetime | The day we read the provider's pages and found they do not say whether a public IPv4 address is included; empty otherwise. |
| `commitment_months` | numeric | Above 1, the price needs a prepaid term of that many months. |
| `sold_out` | boolean | true when the provider's own page shows the plan as sold out (the repository keeps only orderable plans). |
| `sold_out_since` | datetime | The day the plan was first read as sold out. |
| `promo_only` | boolean | true for a promotion-only plan: a sale with no stated end or renewal price. |
| `checked_at` | datetime | The date the price was last read. |
| `price_source` | string | How it was read: api (the provider's public API), page (its pricing page, by the price run) or manual (by a team member). |
| `source_url` | url | Where the price can be checked. |
| `stackrates_url` | url | The StackRates page that shows the row. |

### `history/<provider>.csv`

| Column | Type | Meaning |
|---|---|---|
| `read_date` | datetime | Day the price was read, YYYY-MM-DD: the UTC day of the price run for api and page reads, the day the team member stated for manual reads. One row per plan per day; two reads on one day keep the later one. |
| `provider` | string | Provider slug, as in /data/prices.csv. |
| `plan_id` | string | Plan identifier, stable across days; a plan the provider replaced gets a new id. |
| `plan_name` | string | The provider's plan name as the catalogue stated it on the read day. |
| `vcpu` | numeric | Virtual CPUs. Empty when the provider does not state it (managed plans sold on visits or sites). |
| `ram_gb` | numeric | RAM in GB. Empty when not stated. |
| `disk_gb` | numeric | Disk in GB included in the price. |
| `term_months` | numeric | Billing term the price needs: 1 = month to month or hourly, 12 = paid yearly. |
| `price_usd_month` | numeric | The regular monthly list price in US dollars. A price billed in euros or pounds is converted at the European Central Bank reference rate the price run read that day. Introductory and sale prices are not used. |
| `price_native` | numeric | The same price in the currency the provider bills in. |
| `currency` | string | USD, EUR or GBP. |
| `ipv4_extra_usd` | numeric | Monthly fee for a public IPv4 address on top of price_usd_month: billed on top by the provider, or, for Hetzner and netcup, which print their prices with the address included, the part of the printed price that is the address. 0 when the price includes one at no separate charge; empty when the plan has no public IPv4 address or the provider's pages do not say. price_usd_month + ipv4_extra_usd is the price a buyer pays for the plan with its address: the price the index counts, in every month. |
| `source_url` | url | The provider page or API the price was read from. |
| `read_method` | string | api = the provider's public API; page = the provider's price page, read by our price run; manual = read by a team member from the provider's page and entered with the date. |
| `stackrates_url` | url | The StackRates page that shows the row (the provider's price history page). |

### `index/vps-price-index.csv`

| Column | Type | Meaning |
|---|---|---|
| `month` | string | Calendar month (UTC) of the reads, YYYY-MM. |
| `plans_compared` | numeric | Plans counted this month (the basket). |
| `median_usd_month_1gb` | numeric | Median monthly price in USD, with the public IPv4 address (list price plus the address fee where billed on top; the price a buyer pays for the plan with its address), of the counted plans with exactly 1 GB of RAM. |
| `median_usd_month_4gb` | numeric | The same for 4 GB of RAM. |
| `median_usd_month_8gb` | numeric | The same for 8 GB of RAM. |
| `change_pct_vs_prev_month` | numeric | Median of per-plan percentage changes against the previous month, plans counted in both; empty for the first month or after a month without reads. |
| `plans_up` | numeric | Plans counted in both months whose price went up. |
| `plans_down` | numeric | Plans whose price went down. |
| `plans_same` | numeric | Plans whose price did not change. |
| `method_version` | string | The method these figures were computed with. |
| `plans_1gb` | numeric | How many plans median_usd_month_1gb is taken from. |
| `plans_4gb` | numeric | How many plans median_usd_month_4gb is taken from. |
| `plans_8gb` | numeric | How many plans median_usd_month_8gb is taken from. |
| `read_days` | numeric | Days in the month with at least one read. |
| `last_read` | datetime | The last read date in the month. |
| `note` | string | Plain-words caveat for the row (partial month, no reads, first month). |
| `median_usd_month_1gb_per_provider` | numeric | Median at 1 GB with one vote per provider: each provider's own median over its counted 1 GB plans, then the median of those. |
| `median_usd_month_4gb_per_provider` | numeric | The same for 4 GB of RAM. |
| `median_usd_month_8gb_per_provider` | numeric | The same for 8 GB of RAM. |
| `providers_1gb` | numeric | How many providers median_usd_month_1gb_per_provider is taken over. |
| `providers_4gb` | numeric | How many providers median_usd_month_4gb_per_provider is taken over. |
| `providers_8gb` | numeric | How many providers median_usd_month_8gb_per_provider is taken over. |
| `stackrates_url` | url | The StackRates page that shows the index. The CSV opens with a "# Source: ..." comment line; skip it with comment="#" in pandas. |

### `index/vps-price-index-plans.csv`

| Column | Type | Meaning |
|---|---|---|
| `month` | string | Calendar month (UTC), YYYY-MM. |
| `plan_id` | string | Plan identifier, as in history/<provider>.csv. |
| `provider` | string | Provider slug. |
| `read_date` | datetime | The plan's latest read in the month: the one the month counts. |
| `vcpu` | numeric | Virtual CPUs. |
| `ram_gb` | numeric | RAM in GB. |
| `disk_gb` | numeric | Disk in GB. |
| `term_months` | numeric | Billing term the price needs, in months. |
| `price_usd_month_all_in` | numeric | The price a buyer pays for the plan with its public IPv4 address, in US dollars a month: the regular list price plus the IPv4 fee where billed on top (Hetzner and netcup print it inside their price). The same rule in every month: price_usd_month + ipv4_extra_usd of the history row on read_date. |
| `price_native_all_in` | numeric | The same in the billed currency. |
| `currency` | string | USD, EUR or GBP. |
| `change_pct_vs_prev_month` | numeric | The plan's change against its price in the month before, in the billed currency; empty when it was not counted then. |
| `stackrates_url` | url | The provider's price history page on StackRates. |

### `year-on-year.csv`

| Column | Type | Meaning |
|---|---|---|
| `provider` | string | Provider slug. |
| `plan_today` | string | The plan's name today. |
| `plan_oct_2025` | string | Its name in the October 2025 capture. |
| `vcpu` | numeric | Virtual CPUs; empty when not stated. |
| `ram_gb` | numeric | RAM in GB; empty when not stated. |
| `currency` | string | The currency the provider prints the price in. |
| `price_oct_2025` | numeric | Monthly price the plan billed on the provider's page as the Wayback Machine archived it (archived_on); a figure struck through beside it is in struck_oct_2025, never here. |
| `price_spring_2026` | numeric | The same from a spring 2026 capture, where one exists. |
| `price_today` | numeric | Monthly list price as read on the dataset date; for a withdrawn plan, the last price read. |
| `change_pct` | numeric | price_today against price_oct_2025, in percent; empty where comparable is "no". |
| `comparable` | string | "yes" when the spec and billing basis are the same then and now; "no" where they changed. |
| `archived_page` | url | The Wayback Machine capture the October 2025 price was read from. |
| `archived_on` | datetime | The capture date. |
| `withdrawn_on` | datetime | The day the provider withdrew the plan, if it has. |
| `struck_oct_2025` | string | A figure the October 2025 capture struck through beside the billed price, with no term stated; a fact, not a price. Empty when there was none. |
| `struck_spring_2026` | string | The same for the spring 2026 capture. |
| `stackrates_url` | url | The StackRates page that shows the row. |

## Licence and citation
CC BY 4.0: credit "StackRates (stackrates.ai)" with a link to https://stackrates.ai/data/.

StackRates VPS price index, Nov 2026: 4 GB median $14.02/mo over 93 plans with 4 GB (506 plans in the index), read 11 Nov 2026, stackrates.ai/vps-price-index/

```bibtex
@misc{stackrates_vps_index_2026_11,
  author       = {{StackRates}},
  title        = {{VPS price index, November 2026}},
  year         = {2026},
  month        = {nov},
  howpublished = {\url{https://stackrates.ai/vps-price-index/}},
  note         = {Data set, method 1, CC BY 4.0. 4 GB median \$14.02/mo over 93 plans with 4 GB (506 plans in the index); read 2026-11-11},
  urldate      = {2026-11-11}
}
```

Updated daily at https://github.com/firststategrowth/vps-prices. Run by StackRates, First State Growth LLC.
