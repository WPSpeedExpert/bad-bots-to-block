# CLAUDE.md

This repository contains curated lists of bots to block for website protection. Used across our Cloudflare WAF rules and nginx origin-level blocking (via Ansible `roles/security/bot_block`).

## Files

- `robots.txt` - Template robots.txt for blocking bots
- `cloudflare-firewall-expression.txt` - Cloudflare WAF expression (lowercase, case-insensitive)
- `bots-to-block.md` - Categorized list of bots with descriptions
- `README.md` - Usage instructions with examples for robots.txt, Apache, Nginx, and Cloudflare

## Conventions

- Bot entries in `cloudflare-firewall-expression.txt` must be lowercase (Cloudflare `contains` is case-insensitive)
- Keep bot lists alphabetically sorted within their sections
- When adding/removing bots, update ALL relevant files (robots.txt, cloudflare expression, bots-to-block.md, README examples)
- Document exclusions in the Notes section of `bots-to-block.md`

## GIT COMMIT RULES (CRITICAL)

- **NEVER** include `Co-Authored-By:` lines in commit messages
- **NEVER** include any AI attribution (no "Generated with Claude", no AI references)
- Commit as the repository owner only
- Commit format: short descriptive message, no AI signatures

## Bot categories

### BLOCKED — AI training crawlers (scrape content, no user-visible traffic back)

These bots crawl sites to train AI models. They consume bandwidth and server resources but never send traffic, citations, or sales. Always block:

`gptbot`, `amazonbot`, `anthropic-ai`, `claudebot`, `applebot-extended`, `google-extended`, `meta-externalagent`, `facebookbot`, `ccbot`, `diffbot`, `cohere-ai`, `ai2bot`, `image2dataset`, `imagesiftbot`, `timpibot`, `omgili`, `petalbot`

### BLOCKED — Bad bots, scrapers, scanners

SEO scrapers, data harvesters, internet scanners, generic HTTP clients, spoofed UAs, regional search engines with no audience value. See `bots-to-block.md` for the full list with descriptions.

### EXCLUDED — AI referral bots (drive traffic and sales)

These bots fire when a human asks an AI about a page. The AI fetches the URL and shows a **clickable citation link** back to the site. Blocking them kills AI-sourced traffic and sales:

- `ChatGPT-User` — OpenAI, fires when ChatGPT user asks about a page
- `OAI-SearchBot` — OpenAI ChatGPT search, shows source links
- `Claude-Web` — Anthropic, fires when Claude user asks about a URL
- `PerplexityBot` — Perplexity.ai, displays sources prominently with click-through
- `YouBot` — You.com, shows AI source citations

**DO NOT add these to any block list.** Clients report sales from AI referral traffic.

### EXCLUDED — SEO tools (some clients use them)

- `AhrefsBot` — Ahrefs backlink/SEO crawler (clients use for SEO audits)
- `SemrushBot` — Semrush SEO crawler (clients use for SEO audits)

These are intentionally excluded from the base list. Clients who don't use these tools can add them per-zone in Cloudflare or per-site in nginx.

### EXCLUDED — Search engines and social previews

- `Googlebot`, `Bingbot`, `DuckDuckBot` — regular search indexing
- `facebookexternalhit` — Facebook link previews (breaks sharing if blocked)
- `Twitterbot` — X/Twitter link previews
- `LinkedInBot` — LinkedIn link previews
- `Slackbot` — Slack link previews
- `Applebot` (without `-Extended` suffix) — regular Apple search, not AI training
