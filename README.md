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

## How to Use

### 1. Robots.txt Configuration
To block problematic bots, you can use the following `robots.txt` template:

```plaintext
# Block specific bots
User-agent: AhrefsBot
User-agent: Amazonbot
User-agent: Barkrowler
User-agent: DotBot
User-agent: MJ12bot
User-agent: SemrushBot
User-agent: GPTBot
User-agent: ClaudeBot
User-agent: Timpibot
User-agent: PetalBot
User-agent: FacebookBot
User-agent: GoogleOther
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
    RewriteCond %{HTTP_USER_AGENT} (AhrefsBot|Amazonbot|Barkrowler|DotBot|MJ12bot|SemrushBot|GPTBot|ClaudeBot|Timpibot|PetalBot|FacebookBot|GoogleOther) [NC]
    RewriteRule .* - [F,L]
</IfModule>
```

#### Nginx Configuration:
For Nginx, add the following to your server block:

```nginx
if ($http_user_agent ~* (AhrefsBot|Amazonbot|Barkrowler|DotBot|MJ12bot|SemrushBot|GPTBot|ClaudeBot|Timpibot|PetalBot|FacebookBot|GoogleOther)) {
    return 403;
}
```

Here’s the updated .md section you asked for, with the GitHub link correctly added:

### 3. Cloudflare WAF Rule
If you are using Cloudflare, you can create a WAF rule to block these bots. Use the following expression:

```plaintext
(http.user_agent contains "AhrefsBot") or
(http.user_agent contains "Amazonbot") or
(http.user_agent contains "Barkrowler") or
(http.user_agent contains "DotBot") or
(http.user_agent contains "MJ12bot") or
(http.user_agent contains "SemrushBot") or
(http.user_agent contains "GPTBot") or
(http.user_agent contains "ClaudeBot") or
(http.user_agent contains "Timpibot") or
(http.user_agent contains "PetalBot") or
(http.user_agent contains "FacebookBot") or
(http.user_agent contains "GoogleOther")
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

