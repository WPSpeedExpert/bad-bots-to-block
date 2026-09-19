# Changelog

All changes to the bot lists, with the reason for each. The policy behind them is in the
[README](README.md#what-we-block-and-what-we-dont): **we block bots that harm the site or server,
and never bots that can bring visitors, leads or sales.** A bot that merely shows up in the logs is
not a reason to block it.

## 2026-09-19 — List cleanup

A QA pass against live traffic across a fleet of production WordPress/WooCommerce servers. Goal:
every entry must still be seen in real traffic and must be worth blocking.

### Removed

- **Empty User-Agent** (`http.user_agent eq ""`, nginx `$http_user_agent = ""`). Webhooks, payment
  callbacks, uptime monitors and other integrations can send requests without a
  User-Agent, so the rule risked blocking legitimate traffic. The volume it caught was well below
  the level at which we block, and any scraper can avoid it by setting a User-Agent.
- **YandexBot, YandexImages, SeznamBot, Baiduspider.** Legitimate search engines (the main ones in
  Russia, the Czech Republic and China). They honour robots.txt, so a site can still limit
  them there. Blocking them removed sites from those search indexes. Sites with no audience in
  those markets can add them per zone. Removed from `robots.txt` as well.
- **BingPreview.** A Microsoft crawler whose current use Microsoft does not document. With no
  evidence of harm and a risk of blocking Microsoft services, it does not meet our bar.
- **expanse.** No longer matches the scanner it targeted: Palo Alto Networks' scanner now sends
  "Cortex-Xpanse", which does not contain "expanse". Volume was negligible either way.
- **anthropic-ai** (WAF and nginx only). Deprecated UA that no longer appears in traffic; the live
  training crawler is `ClaudeBot`. Kept in `robots.txt` as an opt-out token.
- **Dalvik/2.1.0, Java, wp_is_mobile** (documentation only). They were listed in `bots-to-block.md`
  but in no rule. Dalvik and Java match real Android apps and Java-based integrations.

### Decided not to block

- **Meta-WebIndexer.** It indexes pages for Meta AI search, which answers questions inside WhatsApp,
  Instagram, Facebook and Messenger and cites the pages it uses. That can bring clients leads.

### Added

- **Reflectionbot.** Training crawler for Reflection AI's open models: high-volume sweeps, no
  referral traffic, and rapidly growing request volume across production servers.

### Documented as never-block

- Link previews: `SkypeUriPreview`, `MicrosoftPreview` (Teams, Outlook), `TelegramBot`, `Discordbot`.
- Commerce and payment integrations: `facebookcatalog` (shop feed), `meta-externalads`, Stripe
  `MerchantSecurityScanner`. Broad substrings such as `facebook` or `meta-` must never be added.

### Changed

- Cloudflare expression sorted alphabetically.
- README: added "What We Block, and What We Don't"; corrected "Why We Block Bytespider" and
  replaced "Competitive Risk of Blocking AI Bots" with "Does Blocking AI Bots Cost Visibility?",
  both of which contradicted the training-vs-referral policy; Cloudflare section now points to
  `cloudflare-firewall-expression.txt` instead of an outdated inline example; removed a stray line.
- `bots-to-block.md`: added the blocking policy; documented the allowed search engines, WhatsApp
  previews and the empty User-Agent decision; this changelog moved here.

## 2026-07-13

Audit vs. Cloudflare's official AI bot list. Added DeepSeekBot and PanguBot (high-impact training
crawlers not on Cloudflare's named list — matters on sites where Super Bot Fight Mode is off).
Removed Google-Extended and Applebot-Extended from the WAF/nginx expressions: they are
robots.txt-only tokens never sent as a User-Agent, so those rules could never fire (retained in
robots.txt). Corrected allow-list to Claude-User (Claude-Web deprecated). Documented the
Aug-2025 PerplexityBot stealth-crawling incident.

## 2026-04-12

Added Google-CloudVertexBot, GoogleOther and TikTokSpider to align with Cloudflare's AI bot
blocking list.

## 2026-04-06

Added 17 AI training crawlers: GPTBot, Amazonbot, ClaudeBot, anthropic-ai, Applebot-Extended,
Google-Extended, Meta-ExternalAgent, FacebookBot, CCBot, Diffbot, cohere-ai, AI2Bot, Image2dataset,
ImagesiftBot, Timpibot, Omgili, PetalBot. Reason: training bots consume 40–50% of server traffic on
some sites, causing OOM crashes. Referral bots (ChatGPT-User, Claude-Web, PerplexityBot) remain
allowed.

## 2025-05-19

Initial list: 31 bad bots (SEO scrapers, scanners, generic clients, regional search engines).
