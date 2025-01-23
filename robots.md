# Robots.txt Template

## Overview
This repository provides a flexible `robots.txt` template designed for WordPress websites to optimize SEO and reduce server resource usage. The file helps control crawler behavior by specifying which areas of the site can and cannot be accessed.

## Template Content
```plaintext
# Created by: OctaHexa Media LLC
User-agent: *
Crawl-delay: 10

# Block access to sensitive directories
Disallow: /wp-admin/
Disallow: /wp-includes/
Disallow: /wp-content/plugins/
Disallow: /wp-content/cache/
Disallow: /wp-json/

# Block specific URL patterns
Disallow: /*?s=*
Disallow: /*?replytocom=
Disallow: /*&orderby=
Disallow: /*&add-to-cart=
Disallow: /*&utm_source=
Disallow: /*&utm_medium=
Disallow: /*&utm_campaign=

# Allow critical assets
Allow: /wp-admin/admin-ajax.php

Sitemap: https://yourdomain.com/sitemap_index.xml
```

## Key Features
- **`Crawl-delay: 10`**: Instructs compliant bots to wait 10 seconds between requests to reduce server load. Note that:
  - **Googlebot** does not honor `Crawl-delay`. Use [Google Search Console](https://search.google.com/search-console) to manage its crawl rate.
  - **Bingbot**, **Yandex**, and some other bots support this directive and will respect it.

- **Disallow Rules**: Prevents access to sensitive directories and unwanted URL patterns, such as search results (`?s=*`) and query strings (`utm_source`, `replytocom`, etc.).

- **Allow Rules**: Grants access to essential assets like `admin-ajax.php` for proper functionality of WordPress features.

- **Sitemap**: Directs bots to the website’s sitemap for structured crawling.

## Usage Instructions
1. Replace `https://yourdomain.com` in the `Sitemap` field with your actual domain.
2. Place the `robots.txt` file in the root directory of your website (e.g., `https://yourdomain.com/robots.txt`).
3. If using a WordPress plugin (e.g., Yoast SEO), ensure manual `robots.txt` changes are supported.
4. Test your `robots.txt` file using tools like [Google’s Robots Testing Tool](https://www.google.com/webmasters/tools/robots-testing-tool).

## Additional Notes
### What Does `Crawl-delay: 10` Mean?
- The `Crawl-delay` directive tells bots to wait 10 seconds between consecutive requests.
- **Purpose**: Prevents bots from overloading your server by controlling crawl frequency.
- **Compatibility**:
  - **Googlebot** does not support `Crawl-delay`. Adjust crawl rates in Google Search Console instead.
  - **Bingbot** and **Yandex** honor this directive.

### Recommendations
- Regularly monitor server logs to identify bots causing high resource usage.
- Use server-level tools or plugins to block malicious bots if needed.
- Keep your `robots.txt` file updated as your site structure changes.

## Contributing
We welcome contributions to improve this template! Please submit a pull request or open an issue with your suggestions.
