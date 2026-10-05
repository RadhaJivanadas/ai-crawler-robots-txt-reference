# AI crawler robots.txt reference

Practical `robots.txt` examples for controlling AI crawler access.

## Allow ChatGPT Search, block training crawlers

This example allows OpenAI's search crawler while blocking training and answer crawlers from OpenAI, Google, Anthropic, and Perplexity:

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

The specific groups target named crawlers. The final wildcard group leaves ordinary crawlers allowed. More specific `User-agent` rules override the wildcard for the named crawler.

Search crawlers such as OAI-SearchBot retrieve content for citation in answers. Training crawlers such as GPTBot, Google-Extended, and ClaudeBot may use content for model training. The distinction depends on each vendor's declared use.

## Block all named AI crawlers

Use this when the site should remain available to ordinary crawlers but named AI crawlers should not fetch it voluntarily:

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

## What robots.txt does and does not do

**Does:**
- Express a request to cooperative crawlers
- Specify which paths each user agent may fetch

**Does not:**
- Authenticate crawlers or prevent direct requests
- Control indexing by itself. `noindex`, canonicals, and HTTP status still apply.
- Prove whether a crawler actually visited
- Block access at the network or firewall level

`robots.txt` is a public file. Do not put secrets, internal paths, or confidential policy in it.

## Verification checklist

- [ ] File is accessible at `https://yourdomain.com/robots.txt`
- [ ] Syntax is correct, with no extra spaces and valid `User-agent:` and `Disallow:` lines
- [ ] The wildcard group (`User-agent: *`) comes after specific rules
- [ ] Rules match current vendor documentation
- [ ] Server logs confirm that the file is served with the `text/plain` content type

Test at the exact public hostname and protocol your site uses.

## Check matching rules

For a quick comparison of which rule matches each crawler, use the [AI Crawler Access Checker](https://www.firmbeacon.co.uk/tools/ai-crawler-check?utm_source=github&utm_medium=readme&utm_campaign=ai_crawler_reference). It reports the matching rule for selected user agents. It does not verify firewall access, indexing, or actual crawler visits.

## Official documentation

- [OpenAI: OAI-SearchBot](https://platform.openai.com/docs/bots#oai-searchbot)
- [OpenAI: GPTBot](https://platform.openai.com/docs/bots#gptbot)
- [Google: crawlers and fetchers](https://developers.google.com/crawling/docs/crawlers-fetchers/overview)
- [Anthropic: ClaudeBot](https://support.anthropic.com/en/articles/8896518-does-anthropic-crawl-the-web)
- [Google: robots.txt introduction](https://developers.google.com/search/docs/crawling-indexing/robots/intro)

Vendor tokens and policies can change. Check official documentation before deploying to production.
