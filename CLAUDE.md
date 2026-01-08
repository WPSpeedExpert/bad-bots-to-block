# CLAUDE.md

This repository contains curated lists of bots to block for website protection.

## Files

- `robots.txt` - Template robots.txt for blocking bots
- `cloudflare-firewall-expression.txt` - Cloudflare WAF expression (lowercase, case-insensitive)
- `bots-to-block.md` - Categorized list of bots with descriptions
- `README.md` - Usage instructions with examples for robots.txt, Apache, Nginx, and Cloudflare

## Conventions

- Bot entries in `cloudflare-firewall-expression.txt` must be lowercase
- Keep bot lists alphabetically sorted within their sections
- When adding/removing bots, update ALL relevant files (robots.txt, cloudflare expression, bots-to-block.md, README examples)
- Document exclusions in the Notes section of `bots-to-block.md`

## Git Commits

- Do NOT add Co-Authored-By lines or any AI attribution to commits
- Commit as the repository owner only

## Excluded Bots

Some bots are intentionally NOT blocked because they provide business value:
- Search bots (GPTBot, ClaudeBot, PerplexityBot, etc.) - for e-commerce discoverability
- SEO tools (AhrefsBot, SemrushBot) - for SEO insights
- Applebot-Extended, FacebookBot, GoogleOther - may drive traffic
