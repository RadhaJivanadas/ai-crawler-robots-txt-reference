# AI crawler robots.txt reference

Practical `robots.txt` examples for separating AI search access from model training access.

## Allow ChatGPT Search and block training crawlers

This example allows OpenAI's search crawler while blocking several training or answer crawlers. Review each vendor's current documentation before publishing it on a production site.

```text
User-agent: OAI-SearchBot
Allow: /

User-agent: GPTBot
Disallow: /

User-agent: Google-Extended
Disallow: /

User-agent: ClaudeBot
Disallow: /

User-agent: PerplexityBot
Disallow: /

User-agent: *
Allow: /
```

The specific groups target the named crawlers. The final group leaves ordinary crawlers allowed. Add path-specific rules when only part of a site should be available.

## Block the named AI crawlers

Use this pattern when the site should remain available to ordinary crawlers but the named AI crawlers should not fetch it voluntarily.

```text
User-agent: OAI-SearchBot
Disallow: /

User-agent: GPTBot
Disallow: /

User-agent: Google-Extended
Disallow: /

User-agent: ClaudeBot
Disallow: /

User-agent: PerplexityBot
Disallow: /

User-agent: *
Allow: /
```

## Check the result

A `robots.txt` file expresses a request to cooperative crawlers. It does not authenticate the crawler, prevent direct requests, control indexing by itself, or prove that a crawler visited the site. Check the public file after deployment and inspect server or CDN logs separately.

For a quick comparison of matching rules across common crawlers, use the [free AI Crawler Access Checker](https://www.firmbeacon.co.uk/tools/ai-crawler-check?utm_source=github&utm_medium=readme&utm_campaign=ai_crawler_reference). It reports the rule that matches each selected user agent. It does not verify firewall access, indexing, citations or whether a crawler actually fetched a page.

## Important details

- A more specific `User-agent` group is evaluated for the named crawler instead of the wildcard group.
- `Allow: /` does not make a site indexable. `noindex`, authentication, HTTP errors and canonical signals still matter.
- `robots.txt` is public. Do not put secrets, internal paths or confidential policy in it.
- Test the file at the exact public hostname and protocol that your site uses.
- Vendor tokens and policies can change. Keep this reference checked against official documentation.

## Official documentation

- [OpenAI: OAI-SearchBot](https://platform.openai.com/docs/bots#oai-searchbot)
- [OpenAI: GPTBot](https://platform.openai.com/docs/bots#gptbot)
- [Google: crawlers and fetchers](https://developers.google.com/crawling/docs/crawlers-fetchers/overview)
- [Anthropic: ClaudeBot](https://support.anthropic.com/en/articles/8896518-does-anthropic-crawl-the-web)
- [Google: robots.txt introduction](https://developers.google.com/search/docs/crawling-indexing/robots/intro)
