# Bad Bots to Block

## Introduction
This repository contains a curated list of bots that are known to cause excessive server resource usage. By blocking these bots, you can safeguard your website’s performance, bandwidth, and overall stability. These bots include scrapers, crawlers, and automated tools that can overload servers, scrape content, or steal data.

## Why Block Bots?
Aggressive bots can:

- Consume server resources excessively.
- Drain bandwidth and impact legitimate user traffic.
- Lead to website crashes or downtime.
- Scrape your website’s content or data without authorization.

By blocking these bots, you can:

- Protect your server from unnecessary strain.
- Improve website performance for legitimate users.
- Safeguard your content and data.

---

## AI Bots: Training Crawlers vs. Referral Bots

As of 2026, we distinguish between two types of AI bots:

### AI Training Crawlers — NOW BLOCKED

These bots scrape content to train AI models. They consume massive bandwidth and server resources but **never send traffic back**. On some servers, training crawlers account for **40–50% of all traffic**, causing MySQL crashes and site outages.

Blocking training crawlers does NOT affect your visibility in AI search results — that's handled by separate referral bots (see below).

This list includes all bots from [Cloudflare's AI bot blocking list](https://developers.cloudflare.com/bots/concepts/bot/#ai-bots), plus additional training crawlers we've identified independently.

| Bot | Company | Why Block |
|-----|---------|-----------|
| GPTBot | OpenAI | Training crawler (NOT ChatGPT search) |
| ClaudeBot, anthropic-ai | Anthropic | Model training |
| Amazonbot | Amazon | AI training (NOT shopping referrals) |
| Google-CloudVertexBot | Google | Vertex AI training |
| Google-Extended | Google | Bard/Gemini training (NOT search indexing) |
| GoogleOther | Google | Non-search crawler used for AI training |
| Applebot-Extended | Apple | Apple Intelligence training |
| Meta-ExternalAgent, FacebookBot | Meta | Meta AI training |
| CCBot | Common Crawl | Open dataset for many LLMs |
| Diffbot | Diffbot | Sells crawled data to AI companies |
| cohere-ai | Cohere | Enterprise LLM training |
| AI2Bot | Allen Institute | Research crawler |
| PetalBot | Huawei | Search and AI training |
| Image2dataset, ImagesiftBot | Various | ML dataset collection |
| TikTokSpider | ByteDance | TikTok crawler, same concerns as Bytespider |
| Timpibot | Timpi | Decentralized AI training |
| Omgili | Webz.io | Content aggregation for AI |

### AI Referral Bots — ALLOWED (drive traffic and sales)

These bots fire when a **human user asks an AI about your content**. The AI fetches your page and shows a **clickable citation link** back to your site. Blocking them kills AI-sourced traffic and sales.

| Statistic | Source |
|-----------|--------|
| ChatGPT has **800M+ weekly users** (10% of world population) | [Visual Capitalist](https://www.visualcapitalist.com/ai-chatbot-market-share-in-2025/) |
| AI visitors convert **4.4x better** than organic search | [Previsible AI SEO Study 2025](https://almcorp.com/blog/previsible-2025-ai-discovery-report/) |
| ~2% of ChatGPT queries involve shopping (**50M queries/day**) | [SE Ranking](https://seranking.com/blog/ai-traffic-research-study/) |
| Some brands report **5% of leads** from ChatGPT referrals | [Passionfruit](https://www.getpassionfruit.com/blog/what-is-gptbot-and-should-you-block-it) |
| AI referral traffic grew **1,300%** during 2024 holiday season | [Position Digital](https://www.position.digital/blog/ai-seo-statistics/) |
| E-commerce AI penetration increased **10.5x** YoY (2024-2025) | [Similarweb](https://www.similarweb.com/blog/insights/ai-news/ai-referral-traffic-winners/) |

| Bot | Company | Why Allow |
|-----|---------|-----------|
| ChatGPT-User | OpenAI | Fires when user asks ChatGPT — shows citation |
| OAI-SearchBot | OpenAI | ChatGPT search with clickable source links |
| Claude-Web | Anthropic | Fires when user asks Claude — shows citation |
| PerplexityBot | Perplexity | Best conversion rates, citation-driven clicks |
| YouBot | You.com | AI search with source citations |
| DuckAssistBot | DuckDuckGo | Excellent crawl-to-refer ratio (0.3:1) |

### The key distinction

```
Training bot:   Your site → scraped into model weights → NO traffic back, ever
Referral bot:   User asks AI → AI fetches your page → User sees citation → Click → Sale
```

**Block training bots (they only take). Allow referral bots (they send traffic).**

### Why We Block Bytespider

While we allow most AI bots, **Bytespider** (ByteDance/TikTok) is explicitly blocked. Unlike GPTBot, ClaudeBot, or PerplexityBot which power consumer-facing AI search tools that drive traffic back to websites, Bytespider offers no business value to Western e-commerce sites.

#### What is Bytespider?

Bytespider is ByteDance's web crawler, launched in April 2024 to gather training data for **Doubao**, their Chinese-language AI chatbot. Despite being owned by TikTok's parent company, Bytespider does not power TikTok's shopping features or any Western-facing AI product.

#### Why Block It?

| Issue | Details |
|-------|---------|
| **Extreme aggression** | Scrapes at **25x the rate of GPTBot** ([Fortune](https://fortune.com/2024/10/03/bytedance-tiktok-bytespider-scraper-bot/)). One site reported **1.4 million hits in a single day**, consuming 14GB of bandwidth. |
| **Ignores robots.txt** | Does not respect disallow directives. Blocking via robots.txt is ineffective—**WAF blocking is required**. ([Dark Visitors](https://darkvisitors.com/agents/bytespider)) |
| **Zero referral traffic** | Powers Doubao (available only in China). Provides **no traffic, no citations, no business value** to Western websites. |
| **Security concerns** | Makes up **54% of AI-enabled attacks** according to security researchers. Nearly **90% of AI crawler traffic** on some sites comes from Bytespider alone. ([HAProxy](https://www.haproxy.com/blog/nearly-90-of-our-ai-crawler-traffic-is-from-tiktok-parent-bytedance-lessons-learned)) |
| **Server impact** | Can consume **10%+ of server compute capacity**. Requests arrive at 5+ per second, causing performance degradation for real users. ([Hypernode](https://www.hypernode.com/en/blog/how-amazons-web-crawler-could-impact-the-performance-of-your-online-store/)) |

#### How to Block Bytespider

Since Bytespider ignores robots.txt, you must block it at the server or WAF level:

**Cloudflare WAF:**
```plaintext
(http.user_agent contains "bytespider")
```

**Nginx:**
```nginx
if ($http_user_agent ~* "bytespider") {
    return 403;
}
```

**Apache (.htaccess):**
```apache
RewriteCond %{HTTP_USER_AGENT} bytespider [NC]
RewriteRule .* - [F,L]
```

#### Common IP Ranges

Bytespider commonly crawls from these IP ranges (useful for firewall rules):
- `110.249.x.x`
- `111.225.x.x`
- `60.8.x.x`

### Competitive Risk of Blocking AI Bots

If you block AI crawlers but competitors don't, AI systems will only be trained on competitor information. When users ask AI assistants about products in your category, your business won't be mentioned. For e-commerce, visibility in AI responses can directly translate to sales.

---

## How to Use

### 1. Robots.txt Configuration
To block problematic bots, you can use the following `robots.txt` template:

```plaintext
# Block specific bots
User-agent: Barkrowler
User-agent: DotBot
User-agent: MJ12bot
User-agent: FacebookBot
Disallow: /

# Allow critical assets
User-agent: *
Crawl-delay: 10

Sitemap: https://yourdomain.com/sitemap_index.xml
```

Replace `https://yourdomain.com` with your actual domain name.

### 2. Server Configuration

#### Apache (.htaccess):
Add the following to your `.htaccess` file to block these bots:

```apache
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteCond %{HTTP_USER_AGENT} (Barkrowler|DotBot|MJ12bot|FacebookBot) [NC]
    RewriteRule .* - [F,L]
</IfModule>
```

#### Nginx Configuration:
For Nginx, add the following **inside `location / { }`** (not at server level):

```nginx
# AI training crawlers
if ($http_user_agent ~* (gptbot|amazonbot|anthropic-ai|claudebot|applebot-extended|google-cloudvertexbot|google-extended|googleother|meta-externalagent|facebookbot|ccbot|diffbot|cohere-ai|ai2bot|image2dataset|imagesiftbot|tiktokspider|timpibot|omgili|petalbot)) {
    return 403;
}

# Bad bots and scrapers
if ($http_user_agent ~* (barkrowler|blexbot|bytespider|censysinspect|dataforseobot|dataprovider|dotbot|mj12bot|megaindex|velenpublicwebcrawler|peer39_crawler|zoominfobot|orbbot|ioncrawl|go-http-client|python-requests|scrapy|mozlila|bw/1.1|news-please|expanse|internet-measurement|isscyberriskcrawler)) {
    return 403;
}

# Regional search engines
if ($http_user_agent ~* (yandexbot|yandeximages|seznambot|360spider|sogou|baiduspider|bingpreview)) {
    return 403;
}

# Empty user agent
if ($http_user_agent = "") {
    return 403;
}
```

> **Important for CloudPanel users:** The `if` blocks must go **inside `location / { }`**, not at `server { }` level. Server-level placement doesn't work with CloudPanel's proxy_pass architecture. See [CloudPanel bot blocking guide](https://github.com/WPSpeedExpert/bad-bots-to-block) for details.

Here’s the updated .md section you asked for, with the GitHub link correctly added:

### 3. Cloudflare WAF Rule
If you are using Cloudflare, you can create a WAF rule to block these bots. Use the following expression:

```plaintext
(http.user_agent contains "Barkrowler") or
(http.user_agent contains "DotBot") or
(http.user_agent contains "MJ12bot") or
(http.user_agent contains "FacebookBot")
```

You can also find a full and updated Cloudflare firewall expression in this repository:
[Complete Cloudflare Firewall Expression (GitHub)](https://github.com/WPSpeedExpert/bad-bots-to-block/blob/main/cloudflare-firewall-expression.txt)

⸻

## Crawl-Delay
To reduce the frequency of bot requests without fully blocking them, add the following to your `robots.txt`:

```plaintext
User-agent: *
Crawl-delay: 10
```
This instructs bots to wait 10 seconds between requests, reducing server strain.

---

## Monitoring and Maintenance

### How to Identify Bots
You can identify aggressive bots by analyzing your server logs. Use the following command to extract user-agent data:

```bash
grep "GET" /path/to/access.log | awk '{print $1}' | sort | uniq -c | sort -nr | head -20
```
Replace `/path/to/access.log` with the actual path to your access logs.

### Update and Review
1. Monitor server logs regularly to identify new bots causing issues.
2. Update your block list and server configurations as needed.
3. Test changes in a staging environment before deploying them to production.

---

## Additional Resources
- [Robots.txt Example on GitHub](https://github.com/WPSpeedExpert/bad-bots-to-block)
- [Complete Cloudflare Firewall Expression (GitHub)](https://github.com/WPSpeedExpert/bad-bots-to-block/blob/main/cloudflare-firewall-expression.txt)
- [Cloudflare WAF Documentation](https://developers.cloudflare.com/waf/)

---

## License
This repository is licensed under the MIT License. Feel free to use, modify, and share it as needed.

---

## Contact
For questions or suggestions, please contact us at **OctaGorilla**:

- [Visit Our Website](https://octagorilla.com)

