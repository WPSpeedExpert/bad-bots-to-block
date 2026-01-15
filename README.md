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

## AI Search Bots: Why We Don't Block Them

As of 2025, AI-powered search tools like ChatGPT, Perplexity, and Claude are increasingly driving real traffic and sales to e-commerce websites. This repository intentionally **does not block** most AI search bots.

### The Business Case

| Statistic | Source |
|-----------|--------|
| ChatGPT has **800M+ weekly users** (10% of world population) | [Visual Capitalist](https://www.visualcapitalist.com/ai-chatbot-market-share-in-2025/) |
| AI visitors convert **4.4x better** than organic search | [Previsible AI SEO Study 2025](https://almcorp.com/blog/previsible-2025-ai-discovery-report/) |
| ~2% of ChatGPT queries involve shopping (**50M queries/day**) | [SE Ranking](https://seranking.com/blog/ai-traffic-research-study/) |
| Some brands report **5% of leads** from ChatGPT referrals | [Passionfruit](https://www.getpassionfruit.com/blog/what-is-gptbot-and-should-you-block-it) |
| AI referral traffic grew **1,300%** during 2024 holiday season | [Position Digital](https://www.position.digital/blog/ai-seo-statistics/) |
| E-commerce AI penetration increased **10.5x** YoY (2024-2025) | [Similarweb](https://www.similarweb.com/blog/insights/ai-news/ai-referral-traffic-winners/) |

### AI Bots We Allow

| Bot | Company | Why Allow |
|-----|---------|-----------|
| GPTBot, ChatGPT-User | OpenAI | 82% AI market share, dominant search alternative |
| PerplexityBot | Perplexity | Best conversion rates, citation-driven clicks |
| ClaudeBot, Claude-Web | Anthropic | Growing platform, improving referrals |
| Amazonbot | Amazon | Powers Alexa & Rufus shopping assistant |
| GoogleOther, Google-Extended | Google | Google AI Overviews and Gemini |
| Meta-ExternalAgent | Meta | Meta AI features |
| DuckAssistBot | DuckDuckGo | Excellent crawl-to-refer ratio (0.3:1) |
| Applebot-Extended | Apple | Apple Intelligence features |
| PetalBot | Huawei | Huawei search and AI |
| cohere-ai | Cohere | Enterprise AI applications |

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
For Nginx, add the following to your server block:

```nginx
if ($http_user_agent ~* (Barkrowler|DotBot|MJ12bot|FacebookBot)) {
    return 403;
}
```

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

