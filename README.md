# rotating proxies: how IP rotation actually works, what it really costs per page, and when ScraperAPI makes more sense than managing your own pool

Rotating proxies swap your exit IP address automatically — either on every request or after a set time window — so the target site sees many different visitors instead of one hammering it repeatedly. That's the whole trick behind avoiding rate limits and IP bans at scale. The real question most people typing this keyword are asking is narrower: which rotation setup fits their project, and what will it actually cost per month once everything is running. This article walks through how rotation works, the main ways to buy it, and a full cost breakdown of one specific approach — ScraperAPI, a scraping API that handles rotation, CAPTCHAs and JS rendering for you — using its current published plans and pricing rules.

> **Quick take:** if you're a developer or small team scraping roughly 20,000–200,000 pages a month against standard e-commerce and search targets, and you don't want to maintain proxy pools and retry logic yourself, ScraperAPI's Hobby plan ($49/month, 100,000 credits, 20 concurrent threads) is the usual entry point. There's a 7-day free trial with 5,000 credits, no credit card required, so you can measure your real per-page cost before paying anything. 👉 [Start free with 5,000 credits — no card required](https://www.scraperapi.com/?fp_ref=coupons)

## What rotating proxies actually do (and the two rotation modes)

A rotating proxy routes your requests through a pool of IP addresses. Your code talks to one fixed gateway address; behind it, the provider swaps the exit IP. The target site sees a stream of different visitors rather than one IP making thousands of requests an hour.

There are two rotation modes, and picking between them matters more than pool size for most projects:

- **Per-request rotation** — every request gets a fresh IP. This is the default for high-volume scraping where you don't need continuity between requests.
- **Sticky sessions** — the same IP stays pinned for a configurable window, usually 30 seconds to 30 minutes on most providers. You typically opt in by appending a session ID to your proxy username. This matters when a target site expects cookie continuity across pages.

One practical sizing rule of thumb from proxy-industry benchmarking: allocate roughly one IP per target domain per 50–100 requests per hour. If your monthly request volume is under 10,000, pool size rarely matters at all — the cheap options will do fine. Above that threshold, pool size and provider tier start deciding whether you get blocked.

## Two ways to buy rotation: raw proxies vs. a scraping API

You can buy rotating proxies in two fundamentally different forms.

**Raw rotating proxy pools** (Bright Data, Smartproxy/Decodo, Oxylabs, SOAX, IPRoyal, NetNut and similar) bill by bandwidth — roughly $4 to $12 per GB depending on the provider and pool type. You get the IP pool, but you still write your own rotation logic, retry handling, header management, and CAPTCHA solving. Bright Data has the largest pool (400M+ IPs across 195+ countries, pay-as-you-go from $4), Smartproxy sits around $8.50/GB with 65M+ IPs and a Site Unblocker API for hard targets, Oxylabs charges around $12/GB but bundles a managed Web Scraper API, IPRoyal is the budget pick at about $7/GB with a smaller 8M+ pool, and NetNut specializes in long sticky sessions (hours, not minutes) for ad verification and logged-in portals.

**Scraping APIs** like ScraperAPI bill by request credits instead of bandwidth. You send a URL to a single endpoint; the service routes it through a rotating pool of 40M+ residential and datacenter IPs across 50+ countries, optionally renders JavaScript with a headless browser, solves CAPTCHAs, manages headers and browser fingerprints, and returns clean HTML — or parsed JSON for structured endpoints covering Amazon, Google Search/Shopping and Walmart. One independent reviewer who ran production scrapers on it for a year reported success rates "hover around 95-99% for most targets" and noted that ScraperAPI "handles React/Vue/Angular SPAs consistently," unlike cheaper competitors whose JS rendering breaks on half the sites.

The trade is straightforward: raw proxies cost less per request at very high volumes but consume engineering time; a scraping API costs more per page at scale but you skip building and maintaining the whole proxy infrastructure. ScraperAPI itself reports usage by over 10,000 brands (including Deloitte, Sony and Alibaba) processing 36 billion API requests monthly, with a 4.9/5 Capterra rating for documentation ease of use.

## ScraperAPI's full plan lineup (current official pricing)

Every plan includes the same core feature set: JS rendering, premium residential and mobile IPs, automatic CAPTCHA and anti-bot handling, JSON auto-parsing, custom headers and sessions, automatic retries, unlimited bandwidth, a 99.9% uptime guarantee, and structured-data endpoints. What changes between tiers is credits, concurrency, geotargeting scope, and a couple of billing options explained below.

| Plan | Monthly price | Credits/month | Concurrent threads | Geotargeting | Get started |
| --- | --- | --- | --- | --- | --- |
| Free | $0 | 1,000 | 5 | None | [ Try free](https://www.scraperapi.com/?fp_ref=coupons) |
| Hobby | $49/mo ($44.10 annual) | 100,000 | 20 | US & EU only | [ Start Hobby](https://www.scraperapi.com/?fp_ref=coupons) |
| Startup | $149/mo ($134.10 annual) | 1,000,000 | 50 | US & EU only | [ Start Startup](https://www.scraperapi.com/?fp_ref=coupons) |
| Business | $299/mo ($269.10 annual) | 3,000,000 | 100 | 50+ countries | [ Start Business](https://www.scraperapi.com/?fp_ref=coupons) |
| Scaling | $475/mo ($427.50 annual) | 5,000,000 | 200 | 50+ countries | [ Start Scaling](https://www.scraperapi.com/?fp_ref=coupons) |
| Professional | $975/mo ($877.50 annual) | 10,500,000 | 300 | 50+ countries | [ Start Professional](https://www.scraperapi.com/?fp_ref=coupons) |
| Advanced | $1,975/mo ($1,777.50 annual) | 21,500,000 | 500 | 50+ countries | [ Start Advanced](https://www.scraperapi.com/?fp_ref=coupons) |
| Enterprise | Custom | 22M+ | 500+ | 50+ countries | [ Contact sales tier](https://www.scraperapi.com/?fp_ref=coupons) |

Annual billing is 10% off across the board, applied automatically when you switch billing cycles — no promo code involved. On the Professional plan that's roughly $97.50 back per month. Every paid plan also has a Start Trial button, and the free tier requires no credit card, so there's no financial risk in testing first. 👉 [See the full plan comparison and current annual prices](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## The part most reviews skip: your real cost per page

Here's the detail that matters more than any headline price: ScraperAPI bills in **credits**, and one credit is the cost of a *standard* page. Harder sites cost more by design — Amazon costs 5 credits, Google and Bing (including subdomains) cost 25, LinkedIn costs 30. Feature toggles add on top: `render=true` (headless browser) adds 10 credits, `premium=true` adds 10, `ultra_premium=true` adds 30, and sites behind Cloudflare, Datadome or PerimeterX protection add 10 credits per request when bypassed.

The combinations aren't linear. Premium plus rendering costs 25 credits, not 20. Ultra-premium plus rendering costs **75 credits** — nearly double what adding the two numbers separately would suggest.

What does that do to your real cost per thousand pages? Using the published rates:

| Plan | Simple HTML (1 cr) | JS rendering (10 cr) | E-commerce (5 cr) | SERP (25 cr) | Ultra-premium + JS (75 cr) |
| --- | --- | --- | --- | --- | --- |
| Hobby ($49) | $0.49 | $4.90 | $2.45 | $12.25 | $36.75 |
| Startup ($149) | $0.15 | $1.49 | $0.75 | $3.73 | $11.18 |
| Business ($299) | $0.10 | $1.00 | $0.50 | $2.49 | $7.48 |
| Scaling ($475) | $0.10 | $0.95 | $0.48 | $2.38 | $7.13 |

Same $49 Hobby plan: 100,000 credits buys 100,000 simple HTML pages, or about 1,333 pages if every request needs ultra-premium proxies plus JavaScript rendering behind Cloudflare protection. That's a 75x spread in what the same subscription delivers, and it's why budgeting against the credit count on the pricing page is a mistake — budget against your real per-page cost instead.

You can do that measurement for free before committing. The dashboard includes a Domain Cost Estimator that tells you the exact credit cost for any URL, and you can set a `max_cost` per scrape so a single request can never blow past a limit you define. The sensible workflow: sign up for the free trial, run a batch of your actual target URLs, check the per-URL costs, then multiply out to your monthly volume and pick the plan that fits.

## Hidden limits that affect which plan you pick

A few restrictions live below the headline feature list, and two of them directly decide which tier you should buy.

**Geotargeting is locked to US & EU on Hobby and Startup.** If you need `country_code` targeting beyond the US and EU — even one specific country elsewhere — you need Business ($299) or above, which unlocks all 50+ countries. This single line quietly makes the Hobby plan a non-starter for some projects regardless of volume.

**How overages are handled depends on your tier.** When credits run out, scraping pauses until renewal by default. Hobby, Startup, Business, Scaling and Professional users can turn on auto-upgrade to jump to the next plan automatically. But **pay-as-you-go** — continuing past your limit at a fixed per-credit rate under a monthly spending cap — is only available on Scaling and above. On Hobby through Business, hitting your cap mid-month means stopping, period (or manually upgrading). Also worth knowing: credits don't roll over; unused credits expire at the end of each billing cycle, though annual plans can renew early for a fresh batch.

**Concurrency is a hard ceiling, enforced cheaply.** Requests beyond your plan's thread limit get a 429 response — turned away, not queued, and not charged. Fine for batch jobs, a real problem for time-sensitive monitoring where a delayed response is a missed price.

**Sessions last about an hour.** The `session_number` parameter keeps the same IP for consecutive requests, but it's designed for single scraping sessions, not long-lived logins. If your use case is ad verification, account management or anything needing a stable IP for hours or days, a rotating scraping API is the wrong tool — dedicated residential or mobile providers built for sticky sessions fit better.

**Support tiers differ.** Hobby and Startup get email/ticket support only; the higher tiers include priority support, a dedicated team, and Slack support.

## Which ScraperAPI plan fits which situation

- **Just exploring or running tiny test jobs:** the Free plan (1,000 credits, 5 concurrent). Use it to run the Domain Cost Estimator against your real targets.
- **Solo developer, price monitoring or SERP tracking, ~20K–100K pages/month of standard targets:** Hobby at $49. At $0.49 per thousand simple pages it's genuinely affordable, and $49 is less than the developer-hours you'd spend wrestling proxies and CAPTCHAs yourself.
- **1M+ credits needed but still US/EU-focused targets:** Startup at $149, where simple pages drop to $0.15 per thousand.
- **Any non-US/EU geotargeting, or ~100K+ concurrency-sensitive jobs:** Business at $299 unlocks all 50+ countries and 100 threads. This is the practical floor for international work.
- **Production pipelines needing overage headroom:** Scaling at $475 is the first tier with pay-as-you-go, so a traffic spike doesn't halt your pipeline mid-month.
- **Very high volume on heavily-protected targets:** be honest with yourself here. At daily volumes of 50K+ requests with rendering enabled, raw residential proxies billed by bandwidth (roughly $4–$12/GB from providers like Bright Data or IPRoyal) plus your own rendering cluster will cost less per page — you're trading money for engineering time in the other direction.

One candid limitation against competitors: ScraperAPI doesn't let you pick individual IPs or ASNs, and session persistence caps out around an hour. Bright Data offers a much larger pool (400M+) and broader product lineup at enterprise scale, and NetNut's multi-hour sticky sessions exist precisely for the workflows ScraperAPI's short sessions can't serve. Neither advantage matters for the core use case — pulling pages off the open web without building infrastructure — but they matter if your project is those things.

## FAQ

**Do rotating proxies guarantee I won't get blocked?**
No. Rotation reduces per-IP fingerprinting and repeat-visit detection, but success rates depend on target difficulty, pool quality and how expensive a proxy tier you're willing to spend. One year-long independent review of ScraperAPI reported 95–99% success on most targets; heavily guarded sites consume more credits per request precisely because they need the premium pool.

**How many rotating proxies do I need?**
Rule of thumb: one IP per target domain per 50–100 requests/hour. Under 10,000 monthly requests, pool size barely matters; above that, it's a deciding factor.

**What's the difference between rotating proxies and sticky sessions?**
Per-request rotation gives every request a new IP. Sticky sessions pin one IP for a set window (30 seconds to 30 minutes typically; up to hours on providers like NetNut) so cookies and logins survive across pages.

**Is there a free way to test ScraperAPI?**
Yes — 7 days with 5,000 credits, no credit card, plus a permanent free plan with 1,000 credits/month at 5 concurrent connections. If you need more test credits, support can grant additional ones.

**Do unused credits roll over?**
No. They expire at the end of each billing cycle. Annual plans can renew early for a fresh batch, which is the official workaround.

## Before you pick a plan

Run this two-minute check first: grab your five most common target URLs, put them through the Domain Cost Estimator on the free trial, and multiply by your monthly volume. If simple pages dominate, the Hobby plan at $49 covers a lot of ground; if you're staring at 25–75 credit pages, the math will push you toward Startup or Business, or possibly toward a raw-proxy setup instead. And if your targets are all outside the US and EU, skip straight past Hobby and Startup — geotargeting makes the decision for you. 👉 [Start the 7-day free trial and measure your real per-page cost](https://www.scraperapi.com/?fp_ref=coupons)
