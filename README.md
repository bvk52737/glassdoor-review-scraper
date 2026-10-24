# glassdoor review scraper: How to Pull Employee Reviews Without Getting Blocked (Plus the Real Cost Math)

Scraping Glassdoor reviews is doable, but the public API is effectively gone. Glassdoor's legacy partner API stopped issuing new developer keys years ago, and what's left is enterprise-partner-only with privately negotiated pricing. That's why most people searching for a glassdoor review scraper in 2026 land on one of three options: open-source scripts, pre-built scrapers on an actor marketplace, or a scraping API that handles the anti-bot work for you.

This article walks through why Glassdoor is uniquely annoying to scrape, what each approach actually costs, and where a general-purpose API like ScraperAPI fits in — with real plan prices, credit rules, and the fine print most comparison posts skip.

> **Quick take:** For developers who want reviews as JSON from a simple API call — and who'd rather outsource proxies, CAPTCHAs, and rendering than maintain them — ScraperAPI's Hobby plan at $49/month (or $44.10/month billed annually) covers 100,000 requests with 20 concurrent threads. Start with the free tier: 1,000 credits/month forever, plus 5,000 bonus credits in your first 7 days, no credit card needed. 👉 [Check ScraperAPI's plans and start the free trial](https://www.scraperapi.com/?fp_ref=coupons)

## Why scraping Glassdoor is harder than scraping most sites

Glassdoor sits on millions of employee-submitted salary figures, company reviews, star ratings, and interview reports — data no public API and no government survey can match. HR teams use it to set pay bands against the live market, employer-brand teams track rating trends and attrition reasons, and competitive-intelligence teams mine reviews to read a rival's culture. The dataset is valuable, and Glassdoor guards it accordingly.

Four walls stand between you and the review pages:

1. **The "give to get" login wall.** Much of the richest review and salary depth is gated behind a prompt to sign in or contribute first. A logged-out scraper only sees the shallow layer.
2. **Aggressive anti-bot protection.** Glassdoor fronts its pages with a challenge layer that fingerprints TLS handshakes, header order, and request cadence. Datacenter IPs get blocked quickly, and plain `requests` or Scrapy scripts get flagged within a handful of hits.
3. **The sign-in overlay and rate limits.** Even on public pages, a modal overlay obscures content, and any IP requesting too fast gets throttled. A real scrape needs rotating residential proxies plus a way to dismiss that overlay.
4. **JavaScript rendering and layout churn.** Parts of the page hydrate client-side, so a raw HTML fetch can silently miss fields. Glassdoor also reshuffles its markup periodically, breaking selector-based scrapers.

The hard part isn't parsing the reviews. It's *reaching* the page — unblocked, un-overlaid, and fully rendered. Every tool you'll evaluate is really just a different answer to that one problem.

## Your three realistic options

**Open-source scripts.** GitHub hosts several Glassdoor review scrapers — Python/Scrapy and Go versions that paginate through review pages and dump results into a CSV or JSON file. One widely used Python scraper runs at roughly 1.5 seconds per review. The code is free, but you inherit every problem in the list above: you supply your own proxies, handle the overlay, fix selectors when Glassdoor changes its markup, and re-test after every layout change. For a one-off research project with a small budget, that trade can make sense. For anything recurring, maintenance becomes the real cost.

**Pre-built marketplace scrapers.** Apify's marketplace hosts dedicated Glassdoor actors. The most full-featured one pulls reviews (including all six sub-ratings: work-life balance, culture and values, career opportunities, compensation and benefits, senior leadership, and diversity and inclusion), job listings, salary percentiles, and interview questions, with sentiment scoring and an HTML report built in. Salaries, interviews, and company profiles are add-ons, off by default. Apify's plans start around $29/month plus usage, and a separate reviews-only actor charges $0.01 per review. You write no code and maintain nothing — which is exactly why you pay more per record.

**A scraping API like ScraperAPI.** You send the Glassdoor URL to a single API endpoint; the service renders JavaScript, rotates residential proxies, solves CAPTCHAs, and returns the raw page for you to parse. You own the parsing logic (so you control exactly which fields you keep), but you don't own the bypass machinery. Plans start at $49/month, and you can test the whole thing for free first.

If you want structured, ready-made review rows and don't want to touch code at all, the Apify route genuinely fits better — that's a fair thing to say. If you're a developer building reviews into your own pipeline and want parsing control without proxy headaches, keep reading.

## ScraperAPI plans: the full table

Here's everything currently shown on the official pricing page, including the free tier:

| Plan | Monthly price (annual billing) | Credits | Concurrency | Geotargeting | Get it |
| --- | --- | --- | --- | --- | --- |
| Free | $0 | 1,000/month + 5,000 trial credits (7 days) | 5 connections | — | [ Start free, no credit card](https://www.scraperapi.com/?fp_ref=coupons) |
| Hobby | $49 ($44.10) | 100,000 | 20 threads | US & EU only | [ Get Hobby](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | $149 ($134.10) | 1,000,000 | 50 threads | US & EU only | [ Get Startup](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | $299 ($269.10) | 3,000,000 | 100 threads | Country-level | [ Get Business](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling | $475 ($427.50) | 5,000,000 | 200 threads | Country-level | [ Get Scaling](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Professional | $975 ($877.50) | 10,500,000 | 300 threads | Country-level | [ Get Professional](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Advanced | $1,975 ($1,777.50) | 21,500,000 | 500 threads | Country-level | [ Get Advanced](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | Custom quote | 22,000,000+ | 500+ threads | Country-level | [ Contact sales](https://www.scraperapi.com/pricing/?fp_ref=coupons) |

Annual billing takes 10% off every paid plan. Every tier — including Hobby — includes the same feature set: JS rendering, premium residential and mobile IPs, advanced anti-bot bypassing, JSON auto-parsing, the DataPipeline, full crawler access, custom headers and sessions, automatic retries, and unlimited bandwidth. The plans differ only in volume, concurrency, and geotargeting depth.

A few differences that don't show up in the table but matter:

- **Hobby and Startup are geotargeting-limited.** If you need Glassdoor reviews from country-specific domains beyond the US and EU, Hobby and Startup cap you at US & EU. Business and above unlock country-level geotargeting.
- **Hobby's analytics dashboard only shows the last 30 days.** Business and up get unlimited history. Irrelevant for most projects, annoying for long-running monitoring jobs.
- **Going over your concurrency limit costs nothing.** ScraperAPI returns a 429 and asks you to retry within your plan's thread count — the request is turned away rather than queued, with no extra charge. You just throttle your loop.

## The credit math most articles skip

This is where the real cost of a glassdoor review scraper lives, and it's the part generic comparisons leave out.

ScraperAPI bills by request, not bandwidth. A standard page costs 1 credit — but harder sites cost more because the service builds custom bypass logic for them. Amazon costs 5 credits per request, Google and Bing cost 25, LinkedIn costs 30. Sites behind bot protection like Cloudflare, Datadome, or PerimeterX add 10 credits per request when the bypass kicks in.

Glassdoor isn't on the published special-pricing list in the material reviewed here, which means its exact per-request cost should be checked before you size a plan. Two tools make that easy:

- **The Domain Cost Estimator** in your dashboard shows the exact credit cost for any URL you plan to scrape.
- **`max_cost`** lets you cap the spend on any single request, so one unexpectedly expensive page can't blow through your credits.

Given Glassdoor's Cloudflare-fronted pages, plan conservatively: if the anti-bot surcharge applies, each review page could cost 11 credits instead of 1. At 11 credits per request, Hobby's 100,000 credits covers roughly 9,000 page requests per month; Startup's million covers about 90,000. Check the estimator first, then pick the tier — this single step is the difference between a plan that lasts the month and one that runs dry in week two.

## What happens when you run out of credits

Most subscription tools just stop when you hit your limit. ScraperAPI gives you three ways to keep going, and knowing which one applies to your plan changes the real-world cost:

1. **Auto-upgrade** (Hobby, Startup, Business, Scaling, Professional): opt in, and when credits run out, your account moves to the next tier automatically — usually at a better price per credit. Useful if your volume grows mid-project; risky if you didn't mean to spend more.
2. **Pay-As-You-Go** (Scaling, Professional, Advanced, Enterprise): keep scraping past your limit at a fixed per-credit rate, up to a monthly spending cap you set. The cap is a limit, not a pre-charge — service stops at the cap unless you change it on the billing page.
3. **Annual plan early renewal**: use up your yearly credits early and re-subscribe on your next billing date for a fresh batch.

If none of that appeals, the default behavior is simply pausing until credits renew. And if the service doesn't work for your target sites, the docs are explicit: **a 7-day no-questions-asked refund**, handled through support. You can also cancel anytime from the dashboard at no charge. That refund window pairs nicely with the 5,000-credit free trial — you can validate Glassdoor access against your real targets before a single dollar is at risk. 👉 [Start the 7-day trial with 5,000 free credits](https://www.scraperapi.com/?fp_ref=coupons)

## Making the actual API calls

Getting reviews through ScraperAPI is a one-line change from any HTTP client. The core pattern:

1. Sign up and grab your API key from the dashboard.
2. Point the API at a Glassdoor company reviews URL (the format is `glassdoor.com/Reviews/{Company}-Reviews-E{ID}.htm`), with `render=true` so JavaScript-hydrated content loads.
3. Parse the returned HTML for the review fields: title, pros, cons, date, overall rating, sub-ratings, reviewer role, and employment status.
4. Paginate and rate-limit on your side, respecting your plan's concurrency cap.

Because you own the parser, you decide which fields survive — nobody charges you per record. The trade-off versus a marketplace actor: when Glassdoor reshuffles markup, you fix the selectors. When the page won't load, that's the API's problem, not yours. For teams that already have parsing code from a previous Glassdoor project, that division of labor is usually the cheapest option available.

One caution worth repeating: Glassdoor's terms of service prohibit automated collection, and employee reviews are personal data. US courts have held that scraping publicly visible pages doesn't violate the CFAA (the *hiQ v. LinkedIn* line of cases), but breaching terms of service is a separate contractual matter, and bulk-collecting review text under GDPR's "legitimate interest" basis is contested. Stick to publicly visible reviews, collect the minimum fields you need — rating, pros, cons, date — and get counsel before any commercial-scale collection.

## How the main options compare

|  | ScraperAPI | Apify Glassdoor actor | Open-source scripts |
| --- | --- | --- | --- |
| What you get | Raw rendered page via API; you parse | Structured JSON rows, ready to use | Whatever the script returns; you fix it |
| Starting cost | Free tier; $49/mo paid | ~$29/mo + usage; or ~$0.01/review | Free |
| Maintenance | None (bypass handled) | None (actor maintained) | Ongoing — selectors, proxies, overlays |
| Parsing control | Full | Actor-defined fields | Full |
| Anti-bot handling | Built in (+10 credits on protected pages) | Built in, residential proxies required | You build it |

Notice what's deliberately absent from that table: purchase links. The Apify actor is the right call if you want zero code and don't mind paying per record; scripts are right for a zero-budget one-off. The comparison is there so you can rule options out, not to send you anywhere.

## FAQ

**Does ScraperAPI have a Glassdoor-specific API?**
No — it's a general-purpose scraping API that renders and unblocks pages, and Glassdoor is one of the sites you can point it at. Parsing is up to you. There's no Glassdoor-only structured endpoint in the current documentation, so if you want pre-parsed review rows with no code, a marketplace actor is the better fit.

**How many reviews can I pull per month on the Hobby plan?**
Depends on Glassdoor's actual per-request credit cost, which you should confirm in the Domain Cost Estimator. If standard 1-credit pricing applied, 100,000 credits would cover 100,000 page requests; if the 10-credit anti-bot surcharge applies to Glassdoor, expect roughly 9,000. Size accordingly.

**What if I go over my limits?**
Concurrency overages are free — you get a 429 and retry. Credit overages pause scraping by default; you can opt into auto-upgrade (Hobby and up) or Pay-As-You-Go (Scaling and up) to continue automatically.

**Is there a refund if it doesn't work?**
Yes — a 7-day no-questions-asked refund through support, plus cancel-anytime with no fee. The 5,000-credit free trial needs no credit card, so you can test Glassdoor access before paying anything.

## Which plan should you actually pick

Match the tier to your review volume, not your ambitions:

- **Just exploring, or one company's reviews for a report** → the Free plan. 1,000 monthly credits plus 5,000 trial credits is genuinely enough to validate the approach.
- **A recurring pipeline pulling a few thousand review pages a month** → Hobby at $49/month. Turn on auto-upgrade only if your volume is unpredictable.
- **Multi-company monitoring, salary benchmarking, or a product built on this data** → Startup ($149) if you stay under ~90,000 monthly requests at worst-case credit costs; Business ($299) if you need country-level geotargeting for non-US/EU Glassdoor domains or unlimited analytics history.
- **Enterprise-scale collection** → Scaling and up, where Pay-As-You-Go means a busy month doesn't stall the pipeline.

Two things to confirm before you click buy: run your target Glassdoor URL through the Domain Cost Estimator so you're sizing against real credit costs, and double-check whether you need geotargeting beyond US & EU — if you do, Hobby and Startup are off the table. Then 👉 [compare all plans and lock in the annual discount](https://www.scraperapi.com/pricing/?fp_ref=coupons).
