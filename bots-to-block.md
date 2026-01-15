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

## Aggressive AI Training Crawlers
These bots aggressively scrape content for AI training but provide zero referral traffic:
- Bytespider (ByteDance/TikTok - 25x more aggressive than GPTBot, ignores robots.txt)

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

## Notes:
- **Applebot-Extended** and **FacebookBot** are not included because they may provide business value.
- **AI search bots** (GPTBot, ChatGPT-User, ClaudeBot, Claude-Web, anthropic-ai, PerplexityBot, cohere-ai, Google-Extended, GoogleOther, Google-CloudVertexBot, Amazonbot, Applebot, Meta-ExternalAgent, PetalBot, DuckAssistBot, etc.) are not blocked because AI-powered search and shopping assistants increasingly drive traffic and sales.
- **Bytespider** (ByteDance) IS blocked because it provides zero referral traffic, is 25x more aggressive than GPTBot, and ignores robots.txt. It powers Doubao (China-only AI), not TikTok shopping.
- **AhrefsBot** and **SemrushBot** are not included as they provide SEO insights that may be valuable for site owners.
- Always keep an eye on Cloudflare Firewall Events to fine-tune new bots appearing.
