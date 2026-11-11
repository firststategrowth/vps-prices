# StackRates price history

Every price StackRates read for every plan, one file per provider, from 2026-10-05 to 2026-11-11 (UTC read dates).
10257 rows, 734 plans, 98 providers. Licence CC BY 4.0.
Written by `app/frontend/scripts/data/build-history.mjs` from the dated record; method 1 (2026-11-06).

Cite as: StackRates (2026). VPS price history, reads 2026-10-05 to 2026-11-11. https://stackrates.ai/data/history/

## Files

- `/data/history/<provider>.csv`: one row per (read_date, plan_id).
- `/data/index/vps-price-index.csv` and `.json`: the monthly VPS price index (method in the JSON).
- `/data/index/vps-price-index-plans.csv`: the plans, read dates and prices behind each month of the index.

## Columns

| Column | Meaning |
|---|---|
| read_date | Day the price was read, YYYY-MM-DD: the UTC day of the price run for api and page reads, the day the team member stated for manual reads. One row per plan per day; two reads on one day keep the later one. |
| provider | Provider slug, as in /data/prices.csv. |
| plan_id | Plan identifier, stable across days; a plan the provider replaced gets a new id. |
| plan_name | The provider's plan name as the catalogue stated it on the read day. |
| vcpu | Virtual CPUs. Empty when the provider does not state it (managed plans sold on visits or sites). |
| ram_gb | RAM in GB. Empty when not stated. |
| disk_gb | Disk in GB included in the price. |
| term_months | Billing term the price needs: 1 = month to month or hourly, 12 = paid yearly. |
| price_usd_month | The regular monthly list price in US dollars. A price billed in euros or pounds is converted at the European Central Bank reference rate the price run read that day. Introductory and sale prices are not used. |
| price_native | The same price in the currency the provider bills in. |
| currency | USD, EUR or GBP. |
| ipv4_extra_usd | Monthly fee for a public IPv4 address on top of price_usd_month: billed on top by the provider, or, for Hetzner and netcup, which print their prices with the address included, the part of the printed price that is the address. 0 when the price includes one at no separate charge; empty when the plan has no public IPv4 address or the provider's pages do not say. price_usd_month + ipv4_extra_usd is the price a buyer pays for the plan with its address: the price the index counts, in every month. |
| source_url | The provider page or API the price was read from. |
| read_method | api = the provider's public API; page = the provider's price page, read by our price run; manual = read by a team member from the provider's page and entered with the date. |
| stackrates_url | The StackRates page that shows the row (the provider's price history page). |

Every CSV here opens with one comment line, `# Source: StackRates, https://stackrates.ai/data/, CC BY 4.0, read <newest read day in the file>`, before the header (the W3C tabular-data comment form; `pandas.read_csv(url, comment='#')` skips it).

Prices are as the providers print them: most EU providers before VAT, US providers before sales tax.

## Provenance by date range

- Before 2026-10-05: no rows. The catalogue imported on 2 Sep 2026 carried prices dated 30 Aug 2026 that the audit of 5 Oct 2026 corrected; they are not reads and are left out.
- 2026-10-05 onward: the daily price run (GitHub Actions, about 04:30 UTC) reads providers' APIs and pages; each run's readings are committed to src/data/live-prices.json, and every committed version is a dated record.
- Contabo, InterServer and netcup turn GitHub's runners away; their pages are read by the same price run on a normal network ("Local price run" commits: the team's laptop from 11 Oct 2026, Node U at about 03:00 UTC from 30 Oct 2026). Until 31 Oct 2026 the run stored those reads under the label "manual" (scripts/pipeline/run.mjs then: `priceSource: source.skipInCi ? 'manual' : reading.source`); they were script reads, and this history labels them with the method the same source has reported since 31 Oct 2026.
- Hand reads: plans priced from the catalogue alone (before a provider's reader existed, or where its site blocks automated readers: Cloudways, AlphaVPS, Time4VPS, some netcup plans) appear on the date the catalogue states for each read, with read_method manual.
- netcup vServer Lite (pico, nano, Lite 1 to 4): read on 2026-10-29 as netcup's German price with 19% VAT divided by 1.19; from 2026-11-06 as netcup's own price without VAT (the plan plus the EUR 0.50 IPv4 address it selects by default). The two methods give the same figure to the cent for five plans; for pico the first gave EUR 1.86 (EUR 2.21 / 1.19 = 1.857) and the second EUR 1.85, with the printed EUR 2.21 unchanged, so the 2026-10-29 row carries the corrected EUR 1.85 (kind reading-method in revisions.csv).
- A struck price at a provider that runs no sale (rule from 2026-11-07): where the provider's page shows a lower price beside a struck-through one with no months stated, and our Deals Desk finds no sale at that provider, price_usd_month and price_native are the price the page bills; the struck figure is not a reference price and is never a row's price. Until 2026-11-07 the run stored the struck figure, so those rows carry the billed figure as a reading-method correction, on every day the provider's page showed the struck figure beside it (from src/data/offer-log.json): Hivelocity pro.md.amd USD 20 to 15 from 2026-10-11, Database Mart Basic USD 12.99 to 9.74 and Professional USD 23.99 to 18.47 on 2026-11-04 to 2026-11-06, Advanced USD 43.99 to 30.79 from 2026-11-07. On the days outside those windows the page billed the higher figure itself, so Database Mart's moves on 2026-11-04 (cuts) and 2026-11-07 (rises for Basic and Professional, a cut for Advanced) are real price moves.
- Plan facts (name, vCPU, RAM, disk, term, IPv4 fee) are the catalogue's on master at the end of the read day. What a plan is (IPv6 only, IPv4 not stated on the provider's pages) is today's catalogue's word for every day, so a later correction applies to the whole history.

All rows by method: api 2015, manual 450, page 7792.

## Coverage by provider

| Slug | Provider | Plans | Rows | First read | Last read | Days read | Days without a read between first and last | Rows by method |
|---|---|---:|---:|---|---|---:|---:|---|
| 20i | 20i | 16 | 64 | 2026-11-08 | 2026-11-11 | 4 | 0 | page 64 |
| accuwebhosting | AccuWebHosting | 4 | 4 | 2026-11-06 | 2026-11-06 | 1 | 0 | manual 4 |
| advinservers | Advin Servers | 5 | 145 | 2026-10-14 | 2026-11-11 | 29 | 0 | api 145 |
| akamai-linode | Akamai Cloud (Linode) | 6 | 216 | 2026-10-05 | 2026-11-11 | 38 | 0 | api 216 |
| alphavps | AlphaVPS | 21 | 63 | 2026-10-29 | 2026-11-06 | 3 | 6 | manual 63 |
| alwyzon | Alwyzon | 6 | 174 | 2026-10-14 | 2026-11-11 | 29 | 0 | page 174 |
| arubacloud | Aruba Cloud | 5 | 133 | 2026-10-14 | 2026-11-11 | 29 | 0 | page 133 |
| atlanticnet | Atlantic.Net | 5 | 160 | 2026-10-11 | 2026-11-11 | 32 | 0 | page 160 |
| aws-lightsail | AWS Lightsail | 11 | 382 | 2026-10-05 | 2026-11-11 | 38 | 0 | api 382 |
| bandwagonhost | BandwagonHost | 4 | 24 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 4, page 20 |
| cherryservers | Cherry Servers | 4 | 128 | 2026-10-11 | 2026-11-11 | 32 | 0 | page 128 |
| civo | Civo | 4 | 116 | 2026-10-14 | 2026-11-11 | 29 | 0 | page 116 |
| cloudcone | CloudCone | 4 | 152 | 2026-10-05 | 2026-11-11 | 38 | 0 | page 152 |
| cloudserver | CloudServer | 4 | 108 | 2026-10-14 | 2026-11-11 | 27 | 2 | page 108 |
| cloudways | Cloudways | 9 | 18 | 2026-10-05 | 2026-11-01 | 3 | 25 | manual 18 |
| contabo | Contabo | 6 | 192 | 2026-10-05 | 2026-11-11 | 33 | 5 | manual 4, page 188 |
| crowncloud | CrownCloud | 6 | 174 | 2026-10-14 | 2026-11-11 | 29 | 0 | page 174 |
| databasemart | Database Mart | 7 | 63 | 2026-11-03 | 2026-11-11 | 9 | 0 | page 63 |
| dedirock | DediRock | 9 | 171 | 2026-10-14 | 2026-11-11 | 29 | 0 | manual 6, page 165 |
| digitalocean | DigitalOcean | 7 | 231 | 2026-10-05 | 2026-11-11 | 33 | 5 | manual 7, page 224 |
| dreamhost | DreamHost | 4 | 132 | 2026-10-10 | 2026-11-11 | 33 | 0 | manual 4, page 128 |
| elementor | Elementor Hosting | 11 | 11 | 2026-11-11 | 2026-11-11 | 1 | 0 | manual 11 |
| ethernetservers | Ethernet Servers | 5 | 30 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 5, page 25 |
| evolutionhost | Evolution Host | 7 | 42 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 7, page 35 |
| evoxt | Evoxt | 5 | 145 | 2026-10-14 | 2026-11-11 | 29 | 0 | page 145 |
| exoscale | Exoscale | 5 | 128 | 2026-10-14 | 2026-11-11 | 29 | 0 | api 127, manual 1 |
| extravm | ExtraVM | 14 | 84 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 14, page 70 |
| fdcservers | FDC Servers | 8 | 48 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 8, page 40 |
| glesys | GleSYS | 5 | 30 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 5, page 25 |
| greencloudvps | GreenCloudVPS | 5 | 165 | 2026-10-10 | 2026-11-11 | 33 | 0 | manual 5, page 160 |
| heartinternet | Heart Internet | 5 | 30 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 5, page 25 |
| hetzner | Hetzner | 10 | 324 | 2026-10-05 | 2026-11-11 | 33 | 5 | api 320, manual 4 |
| hivelocity | Hivelocity | 4 | 128 | 2026-10-11 | 2026-11-11 | 32 | 0 | page 128 |
| hostcolor | Host Color | 13 | 78 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 13, page 65 |
| hosthatch | HostHatch | 5 | 165 | 2026-10-10 | 2026-11-11 | 33 | 0 | manual 5, page 160 |
| hostingcom | Hosting.com | 8 | 148 | 2026-10-11 | 2026-11-11 | 32 | 0 | api 148 |
| hostingde | hosting.de | 5 | 20 | 2026-11-08 | 2026-11-11 | 4 | 0 | page 20 |
| hostinger | Hostinger | 4 | 112 | 2026-10-10 | 2026-11-11 | 28 | 5 | manual 4, page 108 |
| hostnamaste | HostNamaste | 5 | 20 | 2026-11-08 | 2026-11-11 | 4 | 0 | page 20 |
| hostnoc | HostNoc | 7 | 42 | 2026-11-06 | 2026-11-11 | 6 | 0 | api 35, manual 7 |
| hostodo | Hostodo | 5 | 145 | 2026-10-14 | 2026-11-11 | 29 | 0 | page 145 |
| hostpapa | HostPapa | 10 | 10 | 2026-11-11 | 2026-11-11 | 1 | 0 | manual 10 |
| hostus | HostUS | 3 | 87 | 2026-10-14 | 2026-11-11 | 29 | 0 | page 87 |
| hostwinds | Hostwinds | 5 | 165 | 2026-10-10 | 2026-11-11 | 33 | 0 | manual 5, page 160 |
| hyonix | Hyonix | 9 | 36 | 2026-11-08 | 2026-11-11 | 4 | 0 | page 36 |
| iniz | INIZ | 7 | 42 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 7, page 35 |
| interserver | InterServer | 4 | 132 | 2026-10-05 | 2026-11-11 | 33 | 5 | page 132 |
| ionos | IONOS | 5 | 40 | 2026-10-10 | 2026-11-11 | 8 | 25 | manual 10, page 30 |
| kamatera | Kamatera | 3 | 99 | 2026-10-10 | 2026-11-11 | 33 | 0 | manual 3, page 96 |
| khanwebhost | KhanWebHost | 43 | 516 | 2026-10-29 | 2026-11-11 | 12 | 2 | manual 43, page 473 |
| kimsufi | Kimsufi (OVHcloud) | 4 | 116 | 2026-10-14 | 2026-11-11 | 29 | 0 | page 116 |
| kinsta | Kinsta | 5 | 185 | 2026-10-05 | 2026-11-11 | 37 | 1 | page 185 |
| knownhost | KnownHost | 18 | 90 | 2026-11-07 | 2026-11-11 | 5 | 0 | page 90 |
| krystal | Krystal | 6 | 36 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 6, page 30 |
| latitudesh | Latitude.sh | 3 | 73 | 2026-10-14 | 2026-11-11 | 29 | 0 | page 73 |
| liquidweb | Liquid Web | 28 | 274 | 2026-10-11 | 2026-11-11 | 32 | 0 | page 274 |
| liteserver | LiteServer | 4 | 116 | 2026-10-14 | 2026-11-11 | 29 | 0 | page 116 |
| lunanode | LunaNode | 6 | 192 | 2026-10-11 | 2026-11-11 | 32 | 0 | page 192 |
| luxvps | Luxvps | 24 | 144 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 24, page 120 |
| mythicbeasts | Mythic Beasts | 4 | 116 | 2026-10-14 | 2026-11-11 | 29 | 0 | page 116 |
| namescouk | names.co.uk | 4 | 24 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 4, page 20 |
| netart | NetArt | 5 | 40 | 2026-11-04 | 2026-11-11 | 8 | 0 | page 40 |
| netcup | netcup | 11 | 163 | 2026-10-11 | 2026-11-11 | 31 | 1 | manual 12, page 151 |
| nocix | NOCIX | 29 | 29 | 2026-11-06 | 2026-11-06 | 1 | 0 | manual 29 |
| ovhcloud | OVHcloud | 4 | 152 | 2026-10-05 | 2026-11-11 | 38 | 0 | api 152 |
| pawnhoster | Pawnhoster | 3 | 10 | 2026-11-08 | 2026-11-11 | 4 | 0 | page 10 |
| phoenixnap | phoenixNAP | 4 | 112 | 2026-10-14 | 2026-11-11 | 28 | 1 | page 112 |
| prohosting24 | ProHosting24 | 6 | 36 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 6, page 30 |
| quickpacket | QuickPacket | 27 | 108 | 2026-11-08 | 2026-11-11 | 4 | 0 | page 108 |
| racknerd | RackNerd | 9 | 182 | 2026-10-05 | 2026-11-11 | 38 | 0 | manual 5, page 177 |
| raiola | Raiola Networks | 4 | 16 | 2026-11-08 | 2026-11-11 | 4 | 0 | page 16 |
| ramnode | RamNode | 6 | 192 | 2026-10-11 | 2026-11-11 | 32 | 0 | page 192 |
| reliablesite | ReliableSite | 4 | 16 | 2026-11-08 | 2026-11-11 | 4 | 0 | page 16 |
| scaleway | Scaleway | 5 | 160 | 2026-10-11 | 2026-11-11 | 32 | 0 | api 160 |
| securedragon | Secure Dragon | 4 | 104 | 2026-10-15 | 2026-11-11 | 26 | 2 | page 104 |
| servarica | ServaRica | 4 | 116 | 2026-10-14 | 2026-11-11 | 29 | 0 | page 116 |
| serverhost | ServerHost | 5 | 20 | 2026-11-08 | 2026-11-11 | 4 | 0 | page 20 |
| smokyhosts | SmokyHosts | 5 | 20 | 2026-11-08 | 2026-11-11 | 4 | 0 | page 20 |
| softshellweb | SoftShellWeb | 7 | 42 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 7, page 35 |
| spinservers | Spinservers | 9 | 54 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 9, page 45 |
| strato | STRATO | 5 | 20 | 2026-11-08 | 2026-11-11 | 4 | 0 | page 20 |
| swhosting | SW Hosting | 7 | 7 | 2026-11-07 | 2026-11-07 | 1 | 0 | manual 7 |
| tiernet | Tier.Net | 7 | 42 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 7, page 35 |
| time4vps | Time4VPS | 9 | 18 | 2026-10-29 | 2026-11-01 | 2 | 2 | manual 18 |
| ukhost4u | UKHost4u | 6 | 54 | 2026-11-03 | 2026-11-11 | 9 | 0 | page 54 |
| ultahost | Ultahost | 4 | 24 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 4, page 20 |
| ultravps | UltraVPS | 4 | 108 | 2026-10-15 | 2026-11-11 | 27 | 1 | page 108 |
| ultrawebhosting | Ultra Web Hosting | 1 | 9 | 2026-11-03 | 2026-11-11 | 9 | 0 | page 9 |
| upcloud | UpCloud | 5 | 165 | 2026-10-05 | 2026-11-11 | 33 | 5 | manual 5, page 160 |
| veesp | Veesp | 6 | 24 | 2026-11-08 | 2026-11-11 | 4 | 0 | page 24 |
| vexxhost | VEXXHOST | 8 | 48 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 8, page 40 |
| virtono | Virtono | 4 | 116 | 2026-10-14 | 2026-11-11 | 29 | 0 | page 116 |
| vpsbg | VPSBG | 9 | 54 | 2026-11-06 | 2026-11-11 | 6 | 0 | manual 9, page 45 |
| vpsdime | VPSDime | 4 | 128 | 2026-10-11 | 2026-11-11 | 32 | 0 | page 128 |
| vultr | Vultr | 9 | 330 | 2026-10-05 | 2026-11-11 | 38 | 0 | api 330 |
| webdock | Webdock | 3 | 96 | 2026-10-11 | 2026-11-11 | 32 | 0 | page 96 |
| webhosting24 | Webhosting24 | 8 | 192 | 2026-10-15 | 2026-11-11 | 28 | 0 | manual 2, page 190 |
| webhostinguk | WebHosting UK | 6 | 12 | 2026-11-03 | 2026-11-06 | 2 | 2 | manual 6, page 6 |

## Known gaps

- Days from 2026-10-05 to 2026-11-11 with no read at all: none.
- Days with no api or page read (only hand reads, or none): none.
- A plan that could not be read on a day has no row that day; its last good price is never copied forward here (the site's own daily snapshot, src/data/price-history.json, does carry it forward, and is not this file).
- A move of more than 30% is held by the price run for review and is not a read until accepted.
- Providers read by hand only have rows on the days they were read: AccuWebHosting (2026-11-06); AlphaVPS (2026-10-29, 2026-11-01, 2026-11-06); Cloudways (2026-10-05, 2026-10-11, 2026-11-01); Elementor Hosting (2026-11-11); HostPapa (2026-11-11); NOCIX (2026-11-06); SW Hosting (2026-11-07); Time4VPS (2026-10-29, 2026-11-01).
- Days without a read between a provider's first and last read: CloudServer: 2026-11-04 to 2026-11-05; Contabo: 2026-10-06 to 2026-10-10; DigitalOcean: 2026-10-06 to 2026-10-10; Hetzner: 2026-10-06 to 2026-10-10; Hostinger: 2026-10-27 to 2026-10-31; InterServer: 2026-10-06 to 2026-10-10; IONOS: 2026-10-16 to 2026-11-06, 2026-11-08 to 2026-11-10; KhanWebHost: 2026-10-30 to 2026-10-31; Kinsta: 2026-11-10; netcup: 2026-10-21; phoenixNAP: 2026-11-05; Secure Dragon: 2026-10-28 to 2026-10-29; UltraVPS: 2026-10-22; UpCloud: 2026-10-06 to 2026-10-10; WebHosting UK: 2026-11-04 to 2026-11-05.
- Providers with no read on 2026-11-11: WebHosting UK (last read 2026-11-06).
- Plans with no rows in a provider's file before its first read date were not in the catalogue yet (providers were added from 2026-10-05 on).
- Providers with no rows have a file with the header only.
- Providers taken off the catalogue are not published: BuyVM (48 reads, 2026-10-11 to 2026-10-18); HostSailor (4 reads, 2026-10-14 to 2026-10-14). Mark-restricted providers are never published.
- Year-on-year figures from Wayback Machine captures of October 2025 are not reads by us and are not here; they are in /data/year-on-year.csv.

## Corrected rows

30 rows carry a figure the team corrected after the read: 18 in place in the site's own daily record, with a dated commit that says why (kind `daily-record`); 12 because the way the price was read changed while the provider's price did not (kind `reading-method`: the provider's printed price was the same on both reads; the earlier row takes the figure the later method gives). 28 more rows were corrected that way and then restored to the figure as read (kind `reverted`: revisions.csv keeps both entries for each). The row shows the corrected figure, and a corrected figure is never counted as a price move; /data/history/revisions.csv lists each one with the figure as read, the commit that corrected it and its kind (`daily-record`, `reading-method`, or `reverted` for a correction undone, where the row shows the figure as read again).

| Read date | Plan | As read | As corrected | Kind | Corrected by |
|---|---|---|---|---|---|
| 2026-10-05 | cc-4 | USD 7.67 | USD 7.66 | daily-record | d0f213a4 Merge data-fix-entry-price (c62bee2, with origin/master) into graphics-v2: printed price parts, CloudCone, the price study, email opt-in |
| 2026-10-05 | contabo-vps-12 | USD 35.40 | USD 30.00 | daily-record | 778f2ac3 Price corrections: Contabo without Indian GST, UpCloud's own USD prices, Hetzner IPv4 $0.60 |
| 2026-10-05 | contabo-vps-4 | USD 7.79 | USD 6.60 | daily-record | 778f2ac3 Price corrections: Contabo without Indian GST, UpCloud's own USD prices, Hetzner IPv4 $0.60 |
| 2026-10-05 | contabo-vps-6 | USD 10.62 | USD 9.00 | daily-record | 778f2ac3 Price corrections: Contabo without Indian GST, UpCloud's own USD prices, Hetzner IPv4 $0.60 |
| 2026-10-05 | contabo-vps-8 | USD 19.82 | USD 16.80 | daily-record | 778f2ac3 Price corrections: Contabo without Indian GST, UpCloud's own USD prices, Hetzner IPv4 $0.60 |
| 2026-10-05 | upcloud-starter-1 | USD 3.24 | USD 3.50 | daily-record | 778f2ac3 Price corrections: Contabo without Indian GST, UpCloud's own USD prices, Hetzner IPv4 $0.60 |
| 2026-10-05 | upcloud-starter-2 | USD 6.48 | USD 7.00 | daily-record | 778f2ac3 Price corrections: Contabo without Indian GST, UpCloud's own USD prices, Hetzner IPv4 $0.60 |
| 2026-10-05 | upcloud-starter-4 | USD 12.96 | USD 14.00 | daily-record | 778f2ac3 Price corrections: Contabo without Indian GST, UpCloud's own USD prices, Hetzner IPv4 $0.60 |
| 2026-10-05 | upcloud-starter-8 | USD 19.44 | USD 20.00 | daily-record | 778f2ac3 Price corrections: Contabo without Indian GST, UpCloud's own USD prices, Hetzner IPv4 $0.60 |
| 2026-10-05 | upcloud-starter-8-4 | USD 21.60 | USD 24.00 | daily-record | 778f2ac3 Price corrections: Contabo without Indian GST, UpCloud's own USD prices, Hetzner IPv4 $0.60 |
| 2026-10-06 | cc-4 | USD 7.67 | USD 7.66 | daily-record | d0f213a4 Merge data-fix-entry-price (c62bee2, with origin/master) into graphics-v2: printed price parts, CloudCone, the price study, email opt-in |
| 2026-10-07 | cc-4 | USD 7.67 | USD 7.66 | daily-record | d0f213a4 Merge data-fix-entry-price (c62bee2, with origin/master) into graphics-v2: printed price parts, CloudCone, the price study, email opt-in |
| 2026-10-08 | cc-4 | USD 7.67 | USD 7.66 | daily-record | d0f213a4 Merge data-fix-entry-price (c62bee2, with origin/master) into graphics-v2: printed price parts, CloudCone, the price study, email opt-in |
| 2026-10-09 | cc-4 | USD 7.67 | USD 7.66 | daily-record | d0f213a4 Merge data-fix-entry-price (c62bee2, with origin/master) into graphics-v2: printed price parts, CloudCone, the price study, email opt-in |
| 2026-10-10 | cc-4 | USD 7.67 | USD 7.66 | daily-record | d0f213a4 Merge data-fix-entry-price (c62bee2, with origin/master) into graphics-v2: printed price parts, CloudCone, the price study, email opt-in |
| 2026-10-11 | cc-4 | USD 7.67 | USD 7.66 | daily-record | d0f213a4 Merge data-fix-entry-price (c62bee2, with origin/master) into graphics-v2: printed price parts, CloudCone, the price study, email opt-in |
| 2026-10-11 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-11 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-12 | cc-4 | USD 7.67 | USD 7.66 | daily-record | d0f213a4 Merge data-fix-entry-price (c62bee2, with origin/master) into graphics-v2: printed price parts, CloudCone, the price study, email opt-in |
| 2026-10-12 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-12 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-13 | cc-4 | USD 7.67 | USD 7.66 | daily-record | d0f213a4 Merge data-fix-entry-price (c62bee2, with origin/master) into graphics-v2: printed price parts, CloudCone, the price study, email opt-in |
| 2026-10-13 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-13 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-14 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-14 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-15 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-15 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-16 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-16 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-17 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-17 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-18 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-18 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-19 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-19 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-20 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-20 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-21 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-21 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-22 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-22 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-23 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-23 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-24 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-24 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-25 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-25 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-26 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-26 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-27 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-27 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-28 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-28 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-29 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-29 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-29 | netcup-vps-pico | EUR 1.86 | EUR 1.85 (USD 2.06) | reading-method | bc877b4e netcup's own price without VAT, read on its plan page on 6 Nov 2026 (EUR 1.85: the plan plus the EUR 0.50 IPv4 address netcup selects), in place of its German price with 19% VAT divided by 1.19 (EUR 2.21 / 1.19 = 1.857, read 29 Oct 2026 as 1.86); the printed price, EUR 2.21 with VAT, did not change |
| 2026-10-30 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-30 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-10-31 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-10-31 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-11-01 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-11-01 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-11-02 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-11-02 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-11-03 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-11-03 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-11-04 | dbm-basic | USD 12.99 | USD 9.74 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log databasemart|plan|dbm-basic, USD 9.74 beside a struck USD 12.99, started 4 Nov 2026, ended 7 Nov 2026 (USD 12.99 billed on 3 Nov and from 7 Nov: real price moves) |
| 2026-11-04 | dbm-pro | USD 23.99 | USD 18.47 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log databasemart|plan|dbm-pro, USD 18.47 beside a struck USD 23.99, started 4 Nov 2026, ended 7 Nov 2026 (USD 23.99 billed on 3 Nov and from 7 Nov: real price moves) |
| 2026-11-04 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-11-04 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-11-05 | dbm-basic | USD 12.99 | USD 9.74 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log databasemart|plan|dbm-basic, USD 9.74 beside a struck USD 12.99, started 4 Nov 2026, ended 7 Nov 2026 (USD 12.99 billed on 3 Nov and from 7 Nov: real price moves) |
| 2026-11-05 | dbm-pro | USD 23.99 | USD 18.47 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log databasemart|plan|dbm-pro, USD 18.47 beside a struck USD 23.99, started 4 Nov 2026, ended 7 Nov 2026 (USD 23.99 billed on 3 Nov and from 7 Nov: real price moves) |
| 2026-11-05 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-11-05 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-11-06 | dbm-basic | USD 12.99 | USD 9.74 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log databasemart|plan|dbm-basic, USD 9.74 beside a struck USD 12.99, started 4 Nov 2026, ended 7 Nov 2026 (USD 12.99 billed on 3 Nov and from 7 Nov: real price moves) |
| 2026-11-06 | dbm-pro | USD 23.99 | USD 18.47 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log databasemart|plan|dbm-pro, USD 18.47 beside a struck USD 23.99, started 4 Nov 2026, ended 7 Nov 2026 (USD 23.99 billed on 3 Nov and from 7 Nov: real price moves) |
| 2026-11-06 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-11-06 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-11-07 | dbm-advanced | USD 43.99 | USD 30.79 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log databasemart|plan|dbm-advanced, USD 30.79 beside a struck USD 43.99, started 7 Nov 2026 (USD 43.99 billed 3-6 Nov: a real cut on 7 Nov) |
| 2026-11-07 | hivelocity-md | USD 20.00 | USD 15.00 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log hivelocity|vps|pro.md.amd|4|8|80, USD 15 beside a struck USD 20 since 11 Oct 2026 (the log's first day) |
| 2026-11-07 | hivelocity-md | USD 20.00 | USD 20.00 | reverted | c6aac37a reverts 4a0ec4a3: MSA 2.2 ends the discount after the first term; $15 is the first-month price (Legal ruling 7 Nov 2026 evening) |
| 2026-11-08 | dbm-advanced | USD 43.99 | USD 30.79 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log databasemart|plan|dbm-advanced, USD 30.79 beside a struck USD 43.99, started 7 Nov 2026 (USD 43.99 billed 3-6 Nov: a real cut on 7 Nov) |
| 2026-11-09 | dbm-advanced | USD 43.99 | USD 30.79 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log databasemart|plan|dbm-advanced, USD 30.79 beside a struck USD 43.99, started 7 Nov 2026 (USD 43.99 billed 3-6 Nov: a real cut on 7 Nov) |
| 2026-11-10 | dbm-advanced | USD 43.99 | USD 30.79 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log databasemart|plan|dbm-advanced, USD 30.79 beside a struck USD 43.99, started 7 Nov 2026 (USD 43.99 billed 3-6 Nov: a real cut on 7 Nov) |
| 2026-11-11 | dbm-advanced | USD 43.99 | USD 30.79 | reading-method | 4a0ec4a3 the price the page bills is the plan's price at a provider the Deals Desk says runs no sale (black-friday-2026.json status none-today): the figure struck through beside it is not a reference price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)), so storing it as the plan's price was a reading-method error; offer log databasemart|plan|dbm-advanced, USD 30.79 beside a struck USD 43.99, started 7 Nov 2026 (USD 43.99 billed 3-6 Nov: a real cut on 7 Nov) |

Every other row matches the site's daily record (6866 rows checked; 139 rows are on days the daily record does not hold that plan).

## Monthly VPS price index

Medians in USD per month, weighted by plan count (plan count in brackets), then the same medians with one vote per provider (provider count in brackets); change is the median per-plan change against the previous month, in the billed currency.

| Month | Plans | 1 GB | 4 GB | 8 GB | 1 GB, per provider | 4 GB, per provider | 8 GB, per provider | Change % | Up / down / same | Note |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|
| 2026-10 | 293 | 5.00 (37) | 14.00 (59) | 24.99 (51) | 5.00 (30) | 16.50 (46) | 28.69 (40) |  |  | first read 2026-10-05; first month: no earlier month to compare |
| 2026-11 | 506 | 5.00 (49) | 14.02 (93) | 27.48 (82) | 5.05 (41) | 17.00 (73) | 31.99 (65) | 0.00 | 0 / 6 / 282 | month to date: reads up to 2026-11-11; change: 288 plans read in both 2026-10 and 2026-11 |

## Rebuild

From app/frontend:

- `node scripts/data/build-history.mjs --rebuild`: everything from the git record (full clone; weekly on Node U or the laptop).
- `node scripts/data/build-history.mjs`: the daily step after the price run; adds that day's reads, recomputes the index.
- `node scripts/data/build-history.mjs --rebuild --check`: exits 1 if the files differ from a full rebuild.
- `node scripts/data/build-history.mjs --hand-reads=<slug,...>`: adds the catalogue's hand reads of those providers, as the daily step writes them, without the price run's reads; then recomputes the index.
- `node scripts/data/build-history.mjs --index-only`: no new reads; recomputes the index from the files as they are.

## Changes

- 2026-11-06: first release. Method 1.
- 2026-11-06: a "# Source" comment line opens every CSV, and a stackrates_url column (last) links each row to its page.
- 2026-11-06: the Hetzner and netcup IPv4 fee ($0.60, EUR 0.50) in ipv4_extra_usd and in the index's all-in price (it was 0; the 1, 4 and 8 GB medians are unchanged); the weighting stated in the method and the one-vote-per-provider medians added as six columns at the end of the index CSV.
- 2026-11-07: one price rule for every month of the index, the price a buyer pays for the plan with its IPv4 address. Merging the 6 Nov daily refresh had put the Hetzner and netcup rows from 2026-10-05 to 2026-11-05 back to 0 in ipv4_extra_usd and left October's basket without their fee, so 11 of their plans read as rises in November. Those rows carry the fee again (as a full rebuild gives them), and every run now prices every month's basket from its history rows. October's 1, 4 and 8 GB medians are unchanged; November: 0 rose, 0 fell.
- 2026-11-07: reading-method corrections: netcup pico's 2026-10-29 row corrected from EUR 1.86 to EUR 1.85 (the reading method changed, the price did not), so it is never counted as a price move; revisions.csv gains a kind column (last) that tells these from the daily-record corrections.
- 2026-11-07: the hand re-reads of 2026-11-06 (AlphaVPS, 21 plans; netcup vServer Lite, 6 plans) added as that day's rows, read_method manual (`--hand-reads`); they had reached the catalogue between two daily runs. November's basket gains the 6 netcup Lite plans (300 plans; 8 GB median 24.50 to 24.00 because the plans counted differ, 52 to 53); none of the 283 plans read in both months changed its price.
- 2026-11-07: a struck price at a provider that runs no sale is not a price (Legal ruling 7 Nov 2026, rule 2.3; 16 CFR 233.1(a)). 33 rows carry the billed figure as reading-method corrections: Hivelocity pro.md.amd USD 20 to 15 (27 rows from 2026-10-11), Database Mart Basic USD 12.99 to 9.74 and Professional USD 23.99 to 18.47 (3 rows each, 2026-11-04 to 2026-11-06); Database Mart Advanced USD 43.99 to 30.79 applies from 2026-11-07, that day's rows included. Database Mart's cuts on 2026-11-04 are now price moves. The index's medians and its 0 rose, 0 fell are unchanged (Hivelocity pro.md.amd is USD 15 in both months; Database Mart was first read in November). The same day, the 2026-11-06 hand reads of AlphaVPS and netcup re-priced at the 2026-11-06 ECB rate (1.1165; they had the 2026-11-05 rate, 1.1224): netcup pico USD 2.08 to 2.07.
- 2026-11-07: the daily step for 2026-11-07, run before the evening price run: 361 rows of that day's reads at 56 providers, and the catalogue's hand reads of 2026-11-06 at 23 providers (198 rows, read_method manual; 22 of them get their first file, and Known Host its first with that day's reads), which had reached the catalogue between two daily runs. November's basket goes from 300 to 448 plans; its 8 GB median moves from 24.00 to 27.91 and the per-provider 8 GB median from 27.50 to 31.99 because the plans counted differ, not because a price moved: none of the 285 plans read in both months changed its price.
- 2026-11-07: Hivelocity pro.md.amd restored to USD 20 on its 28 rows from 2026-10-11 (kind `reverted` in revisions.csv, beside the earlier reading-method entries, which stay). Its agreement ends discounts after the first term and renews a monthly order at its list price (MSA 2.2, read 2026-11-07), so USD 20 is the price and USD 15 a first-month price; the 4a0ec4a3 correction to USD 15 earlier that day was wrong. The index's medians and its 0 rose, 0 fell are unchanged (pro.md.amd is USD 20 in both months).
