# List of Bots to Block

---

## SEO Crawlers
These bots gather SEO-related data and often sell it to competitors or clients:
- Barkrowler
- BLEXBot
- DataForSeoBot
- DotBot
- MegaIndex
- MJ12bot

---

## Security Scanners
These bots perform security scans or collect server data:
- CensysInspect
- internet-measurement
- ISSCyberRiskCrawler

---

## AI Training Crawlers
These bots scrape website content to train AI/LLM models. They consume bandwidth and server resources but provide zero referral traffic — they never show citations or send visitors back. Blocking them does NOT affect your visibility in AI search results (that's handled by separate referral bots like ChatGPT-User which are intentionally allowed).

This list includes all bots from [Cloudflare's AI bot blocking list](https://developers.cloudflare.com/bots/concepts/bot/#ai-bots), plus additional training crawlers we've identified independently.

- AI2Bot (Allen Institute for AI — research crawler)
- Amazonbot (Amazon — powers Alexa/Rufus AI training, NOT shopping referrals)
- anthropic-ai ‡ (Anthropic — legacy Claude training UA, superseded by ClaudeBot)
- Applebot-Extended † (Apple — Apple Intelligence training opt-out)
- Bytespider (ByteDance/TikTok — 25x more aggressive than GPTBot, ignores robots.txt)
- CCBot (Common Crawl — open dataset used by many LLMs including GPT, LLaMA, etc.)
- ClaudeBot (Anthropic — Claude model training crawler)
- cohere-ai (Cohere — enterprise LLM training)
- DeepSeekBot (DeepSeek — LLM training crawler, zero referral traffic)
- Diffbot (Diffbot — sells crawled data to LLM companies)
- FacebookBot (Meta — web scraping for Meta AI)
- Google-CloudVertexBot (Google — Vertex AI training crawler)
- Google-Extended † (Google — Gemini AI training opt-out, separate from Googlebot search indexing)
- GoogleOther (Google — generic non-search R&D/fetch crawler; returns no referral traffic)
- GPTBot (OpenAI — GPT model training, NOT ChatGPT search)
- Image2dataset (ML research — image dataset collection)
- ImagesiftBot (Hive AI — reverse image search training)
- Meta-ExternalAgent (Meta — Meta AI model training)
- Omgili/Omgilibot (Webz.io — crawls and resells data, including for LLM training)
- PanguBot (Huawei — trains Huawei's PanGu LLM, no referral; counterpart to PetalBot)
- PetalBot (Huawei — Petal Search and AI training. Kept blocked for its crawl volume; it sends Western sites almost no traffic)
- Reflectionbot (Reflection AI — training crawler for its open models; high-volume sweeps, no referral traffic)
- TikTokSpider (ByteDance/TikTok — TikTok content crawler, same concerns as Bytespider)
- Timpibot (Timpi — decentralized AI search training)

† **`Google-Extended` and `Applebot-Extended` are robots.txt-only opt-out tokens.** Neither is ever sent as an HTTP `User-Agent` string — Google crawls under normal Googlebot/Vertex UAs and the token governs *training use* via robots.txt only. A `User-Agent` match on these in a WAF or nginx rule can never fire, so they are listed in `robots.txt` **only** and deliberately excluded from `cloudflare-firewall-expression.txt`. Do not "fix" this by blocking bare `applebot`/`googlebot` — those are search crawlers you want.

‡ **`anthropic-ai` is in `robots.txt` only.** It is a deprecated UA that no longer appears in traffic, so it was removed from the WAF and nginx expressions (2026-09-19). The live training crawler is `ClaudeBot`.

---

## Low-Value Regional Search Engine Crawlers
Search engine crawlers with little audience value for most sites and a poor crawl reputation:
- 360Spider
- Sogou Spider

YandexBot, SeznamBot and Baiduspider are **not** in this section: they are legitimate search engines and are allowed by default (see Notes).

---

## Other Bots and Scrapers
General-purpose scrapers, bad actors, or suspicious user agents:
- BW/1.1
- Dataprovider
- Go-http-client
- IonCrawl
- Mozlila (not to be confused with Mozilla)
- news-please
- Orbbot
- peer39_crawler
- python-requests
- Scrapy
- VelenPublicWebCrawler
- Zoominfobot

---

## Notes

### Our blocking policy: harm, not presence

**We block bots that harm the site or server.** An entry belongs here when the bot hammers servers and consumes real resources — sustained high request volume, cache-busting requests, crashes — or when it is an AI training crawler that only takes content. **A bot that shows up in the logs ten times is not blocked.** Every entry adds false-positive risk; the list is valuable because it is short and current.

**We never block bots that can bring visitors, leads or sales**, even busy ones: AI search and referral bots, search engines, link-preview fetchers, and commerce and payment integrations.

Every addition and removal, with its reason, is recorded in [`CHANGELOG.md`](CHANGELOG.md).

### Why we now block AI training bots

As of April 2026, we distinguish between **training crawlers** (which only take content) and **referral bots** (which send traffic back via citations). Training crawlers consume significant server resources — in one real incident, GPTBot and Amazonbot accounted for 48.9% of a server's total traffic, contributing to MySQL OOM crashes and 22 minutes of downtime.

Blocking training crawlers does NOT reduce your visibility in AI search results. When a user asks ChatGPT about your product, `ChatGPT-User` (not `GPTBot`) fetches your page and shows a citation. These referral bots are intentionally allowed.

### Bots intentionally NOT blocked

**AI referral bots (drive traffic and sales via citations):**
- `ChatGPT-User` — fires when a ChatGPT user asks about a page, shows clickable citation
- `OAI-SearchBot` — ChatGPT search results with source links
- `Claude-User` — fires when a Claude user asks about a URL, shows citation (this is the **live** token; the old `Claude-Web` and `anthropic-ai` UAs are deprecated)
- `Claude-SearchBot` — Anthropic search indexing, low volume
- `PerplexityBot` — Perplexity.ai displays sources prominently with click-through ⚠️ see caveat below
- `meta-externalfetcher` — Meta assistant user-fetch (the referral counterpart to Meta-ExternalAgent)
- `meta-webindexer` — indexes pages for Meta AI search (inside WhatsApp, Instagram, Facebook, Messenger), which cites and links to sources. Allowed because it can bring clients leads
- `YouBot` — You.com AI shows source citations
- `DuckAssistBot` — DuckDuckGo AI with excellent crawl-to-refer ratio

> ⚠️ **PerplexityBot caveat (August 2025).** Cloudflare **de-listed Perplexity as a verified bot** and began actively blocking it after finding that, when sites disallowed `PerplexityBot`, Perplexity switched to an **undeclared stealth crawler** spoofing a normal Chrome/macOS browser UA from unlisted, rotating IPs — an estimated 3–6M disguised requests/day ([Cloudflare](https://blog.cloudflare.com/perplexity-is-using-stealth-undeclared-crawlers-to-evade-website-no-crawl-directives/)). We still allow `PerplexityBot` because its human-initiated fetches do cite and drive click-through, and because **blocking the UA would not stop the stealth crawler anyway** (that requires Cloudflare's bot-score / managed protection, not a User-Agent rule). Site owners who don't value Perplexity referral traffic can add `perplexitybot` per-zone.

**SEO tools (some site owners use these):**
- `AhrefsBot` — Ahrefs backlink/SEO crawler
- `SemrushBot` — Semrush SEO crawler

Site owners who don't use Ahrefs or Semrush can add these per-zone.

**Search engines and link previews:**
- `Googlebot`, `Bingbot`, `DuckDuckBot` — search indexing
- `Applebot` (without `-Extended`) — regular Apple search
- `facebookexternalhit` — Facebook link previews (blocks sharing if blocked)
- `Twitterbot`, `LinkedInBot`, `Slackbot` — social link previews
- `WhatsApp` — WhatsApp link previews (sends `WhatsApp/2.x`)
- `YandexBot`, `SeznamBot`, `Baiduspider` — main search engines in Russia, the Czech Republic and China. They honour robots.txt. Sites with no audience in those markets can add them per zone
- `BingPreview` — Microsoft crawler; Microsoft doesn't document its current use, and there is no evidence of harm

- `SkypeUriPreview`, `MicrosoftPreview` — Microsoft Teams and Outlook link previews
- `TelegramBot`, `Discordbot` — Telegram and Discord link previews

**Commerce and payment integrations — never match these:**
- `facebookcatalog` — Facebook/Instagram shop catalogue feed. Blocking it breaks the shop
- `meta-externalads` — Meta ads integration
- `MerchantSecurityScanner` — Stripe's merchant security scanner

Because of these, never add a broad substring such as `facebook` or `meta-`; match the exact bot token (`meta-externalagent`, `facebookbot`).

**Empty User-Agent — deliberately not blocked.** Webhooks, payment callbacks, uptime monitors and other integrations can send requests with no User-Agent, so the rule risks blocking legitimate traffic. The volume it caught was well below our blocking bar, and any scraper can avoid it by setting a User-Agent. Removed 2026-09-19.

**Generic clients we do not match:** `Dalvik` and `Java` match real Android apps and Java-based services and integrations, so they are not on the list.

### Scope: this is a Cloudflare WAF *custom rule*

This list feeds a Cloudflare **WAF custom rule** (mirrored to nginx), which *supplements* Cloudflare's managed protection. We deliberately do **not** try to duplicate everything Cloudflare's managed rules / Super Bot Fight Mode (SBFM) already catch — the list stays lean and targets the **most problematic UAs**.

The important exception: **some sites cannot enable SBFM**, because it blocks a required service or payment integration (e.g. **Zapier**). On those sites this custom rule is the *only* bot defense. So high-impact training crawlers are included **explicitly** even when Cloudflare also covers them — never assume SBFM is on. Low-value bots that Cloudflare reliably handles are intentionally left out.

Deliberately kept lean — added reactively from logs, not pre-emptively: scraping-as-a-service agents (`FirecrawlAgent`, `ApifyBot`, `Brightbot`) and niche/regional crawlers (`iaskspider`, `Kangaroo Bot`, `ICC-Crawler`).

**Substring matching note:** Cloudflare `contains` is a substring match, so several variants are already absorbed for free — `googleother` catches `GoogleOther-Image`/`-Video`, `omgili` catches `Omgilibot`, `bytespider` catches `Bytespider/*`. No separate entries needed.

### Monitoring
- Always keep an eye on Cloudflare Firewall Events to fine-tune new bots appearing.
- Check server access logs for bots bypassing Cloudflare (direct origin access).
- If a new AI training bot appears, add it to the training crawlers list.

### Changelog

Moved to [`CHANGELOG.md`](CHANGELOG.md).
