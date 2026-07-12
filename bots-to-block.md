# List of Bots to Block

---

## SEO Crawlers
These bots gather SEO-related data and often sell it to competitors or clients:
- Barkrowler
- BLEXBot
- DataForSeoBot
- DotBot
- MJ12bot
- MegaIndex

---

## Security Scanners
These bots perform security scans or collect server data:
- CensysInspect
- Expanse
- internet-measurement
- ISSCyberRiskCrawler

---

## AI Training Crawlers
These bots scrape website content to train AI/LLM models. They consume bandwidth and server resources but provide zero referral traffic — they never show citations or send visitors back. Blocking them does NOT affect your visibility in AI search results (that's handled by separate referral bots like ChatGPT-User which are intentionally allowed).

This list includes all bots from [Cloudflare's AI bot blocking list](https://developers.cloudflare.com/bots/concepts/bot/#ai-bots), plus additional training crawlers we've identified independently.

- AI2Bot (Allen Institute for AI — research crawler)
- Amazonbot (Amazon — powers Alexa/Rufus AI training, NOT shopping referrals)
- anthropic-ai (Anthropic — Claude model training)
- Applebot-Extended (Apple — Apple Intelligence training, separate from regular Applebot)
- Bytespider (ByteDance/TikTok — 25x more aggressive than GPTBot, ignores robots.txt)
- CCBot (Common Crawl — open dataset used by many LLMs including GPT, LLaMA, etc.)
- ClaudeBot (Anthropic — Claude model training crawler)
- cohere-ai (Cohere — enterprise LLM training)
- Diffbot (Diffbot — sells crawled data to LLM companies)
- FacebookBot (Meta — web scraping for Meta AI)
- Google-CloudVertexBot (Google — Vertex AI training crawler)
- Google-Extended (Google — Bard/Gemini AI training, separate from Googlebot search indexing)
- GoogleOther (Google — general-purpose non-search crawler used for AI training)
- GPTBot (OpenAI — GPT model training, NOT ChatGPT search)
- Image2dataset (ML research — image dataset collection)
- ImagesiftBot (Hive AI — reverse image search training)
- Meta-ExternalAgent (Meta — Meta AI model training)
- Omgili/Omgilibot (Webz.io — content aggregation for AI companies)
- PetalBot (Huawei — Huawei search and AI training)
- TikTokSpider (ByteDance/TikTok — TikTok content crawler, same concerns as Bytespider)
- Timpibot (Timpi — decentralized AI search training)

---

## Aggressive or Niche Search Engine Crawlers
Search engine crawlers that are non-essential or add load:
- YandexBot
- SeznamBot
- BaiduSpider
- 360Spider
- Sogou Spider

---

## Other Bots and Scrapers
General-purpose scrapers, bad actors, or suspicious user agents:
- BW/1.1
- Dalvik/2.1.0
- Dataprovider
- Empty user agent
- Go-http-client
- IonCrawl
- Java
- Mozlila (not to be confused with Mozilla)
- news-please
- Orbbot
- peer39_crawler
- python-requests
- Scrapy
- VelenPublicWebCrawler
- wp_is_mobile
- Zoominfobot
- BingPreview

---

## Notes

### Why we now block AI training bots

As of April 2026, we distinguish between **training crawlers** (which only take content) and **referral bots** (which send traffic back via citations). Training crawlers consume significant server resources — in one real incident, GPTBot and Amazonbot accounted for 48.9% of a server's total traffic, contributing to MySQL OOM crashes and 22 minutes of downtime.

Blocking training crawlers does NOT reduce your visibility in AI search results. When a user asks ChatGPT about your product, `ChatGPT-User` (not `GPTBot`) fetches your page and shows a citation. These referral bots are intentionally allowed.

### Bots intentionally NOT blocked

**AI referral bots (drive traffic and sales via citations):**
- `ChatGPT-User` — fires when a ChatGPT user asks about a page, shows clickable citation
- `OAI-SearchBot` — ChatGPT search results with source links
- `Claude-Web` — fires when a Claude user asks about a URL, shows citation
- `PerplexityBot` — Perplexity.ai displays sources prominently with click-through
- `YouBot` — You.com AI shows source citations
- `DuckAssistBot` — DuckDuckGo AI with excellent crawl-to-refer ratio

**SEO tools (some site owners use these):**
- `AhrefsBot` — Ahrefs backlink/SEO crawler
- `SemrushBot` — Semrush SEO crawler

Site owners who don't use Ahrefs or Semrush can add these per-zone.

**Search engines and link previews:**
- `Googlebot`, `Bingbot`, `DuckDuckBot` — search indexing
- `Applebot` (without `-Extended`) — regular Apple search
- `facebookexternalhit` — Facebook link previews (blocks sharing if blocked)
- `Twitterbot`, `LinkedInBot`, `Slackbot` — social link previews

### Monitoring
- Always keep an eye on Cloudflare Firewall Events to fine-tune new bots appearing.
- Check server access logs for bots bypassing Cloudflare (direct origin access).
- If a new AI training bot appears, add it to the training crawlers list.

### Changelog

| Date | Change |
|------|--------|
| 2026-04-12 | Added Google-CloudVertexBot, GoogleOther, TikTokSpider to align with Cloudflare's AI bot blocking list |
| 2026-04-06 | Added 17 AI training crawlers: GPTBot, Amazonbot, ClaudeBot, anthropic-ai, Applebot-Extended, Google-Extended, Meta-ExternalAgent, FacebookBot, CCBot, Diffbot, cohere-ai, AI2Bot, Image2dataset, ImagesiftBot, Timpibot, Omgili, PetalBot. Reason: training bots consume 40-50% of server traffic on some sites, causing OOM crashes. Referral bots (ChatGPT-User, Claude-Web, PerplexityBot) remain allowed. |
| 2025-05-19 | Initial list: 31 bad bots (SEO scrapers, scanners, generic clients, regional search engines) |
