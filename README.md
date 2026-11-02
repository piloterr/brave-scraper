<p align="center">
  <a href="https://www.piloterr.com"><img src="images/banner.png" alt="Brave Scraper API by Piloterr – scrape Brave data as JSON" width="100%"></a>
</p>

# Brave Scraper API

[![Discord](https://img.shields.io/badge/Discord-Join%20the%20community-5865F2?logo=discord&logoColor=white)](https://discord.gg/CFj3ph8aHU) [![Get API key](https://img.shields.io/badge/Piloterr-Get%20a%20free%20API%20key-111827)](https://app.piloterr.com) [![API docs](https://img.shields.io/badge/Docs-OpenAPI-0EA5E9)](https://app.piloterr.com/openapi) [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

The **Piloterr Brave Scraper API** lets you extract public data from Brave as **clean, structured JSON** — no proxies, headless browsers or HTML parsing to maintain. Endpoints available in production:

- **Brave Search API** — Access the Brave search engine results directly through our efficient Piloterr service for reliable data scraping.

Use it for SEO rank tracking, SERP monitoring, ad intelligence, market research or grounding LLM answers with fresh search results.

| Endpoint | Path | Credits | What you get |
|---|---|---|---|
| [Brave Search API](#brave-search-api) | `/v2/brave/search` | 1 | Access the Brave search engine results directly through our efficient Piloterr service for reliable data scraping. |

## Table of contents

- [How it works](#how-it-works)
- [Brave Search API](#brave-search-api)
- [Code examples in other languages](#code-examples-in-other-languages)
- [Practical use cases](#practical-use-cases)
- [Why Piloterr?](#why-piloterr)
- [FAQ](#faq)
- [Related scrapers](#related-scrapers)
- [Support](#support)

## How it works

1. **Create a free Piloterr account** and copy your API key from the [dashboard](https://app.piloterr.com).
2. **Send a `GET` (or `POST`) request** to `https://api.piloterr.com/v2/...` with the header `x-api-key: YOUR_API_KEY`.
3. **Receive structured JSON** — Piloterr handles rendering, proxies, retries and parsing.

```bash
export PILOTERR_API_KEY="your-api-key"
pip install requests        # Python
# TypeScript: Node.js 18+ (native fetch), run with `npx tsx file.ts`
```

> Every endpoint accepts both `GET` (query string) and `POST` (JSON body with the same fields).

## Brave Search API

Access the Brave search engine results directly through our efficient Piloterr service for reliable data scraping.

`GET | POST https://api.piloterr.com/v2/brave/search` · **1 credit per request** · [Full reference](https://app.piloterr.com/openapi)

### Request parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | ✅ | The search query to execute on Brave Search. Supports all standard Brave search operators. |
| `page` | number | — | Page number of results to retrieve. Used for pagination. |
| `gl` | string | — | Country code, for example us, fr, gb, de. |
| `hl` | string | — | Language code, for example en, fr, de. |

### Python example

```python
import json
import os
import requests

response = requests.get(
    "https://api.piloterr.com/v2/brave/search",
    headers={"x-api-key": os.environ["PILOTERR_API_KEY"]},
    params={
        "query": "anthropic",
    },
    timeout=120,
)
response.raise_for_status()
data = response.json()

print(json.dumps(data, indent=2, ensure_ascii=False)[:2000])
```

### TypeScript example

```typescript
type BraveSearch = {
  organic_results?: {
    link?: string;
    title?: string;
    domain?: string;
    snippet?: string;
    position?: number;
    displayed_link?: string;
    snippet_matched?: string[];
  }[];
  search_parameters?: {
    q?: string;
    gl?: string;
    hl?: string;
    page?: number;
    engine?: string;
  };
};

const params = new URLSearchParams({
  "query": "anthropic",
});

const response = await fetch(`https://api.piloterr.com/v2/brave/search?${params}`, {
  headers: { "x-api-key": process.env.PILOTERR_API_KEY! },
});
if (!response.ok) throw new Error(`Piloterr error ${response.status}: ${await response.text()}`);

const data = (await response.json()) as BraveSearch;

console.log(JSON.stringify(data, null, 2).slice(0, 2000));
```

<details>
<summary><b>POST variant</b></summary>

```http
POST https://api.piloterr.com/v2/brave/search
x-api-key: YOUR_API_KEY
Content-Type: application/json

{"query": "anthropic"}
```
</details>

### Output example

Example response from the API reference, abridged — real values depend on the page you scrape.

```json
{
  "organic_results": [
    {
      "link": "https://www.anthropic.com/",
      "title": "Anthropic",
      "domain": "www.anthropic.com",
      "snippet": "Anthropic is an AI safety company. Claude is its assistant.",
      "position": 1,
      "displayed_link": "anthropic.com",
      "snippet_matched": [
        "anthropic"
      ]
    },
    {
      "link": "https://docs.anthropic.com/",
      "title": "Anthropic documentation",
      "domain": "docs.anthropic.com",
      "snippet": "Documentation for the Claude API and the Anthropic platform.",
      "position": 2,
      "displayed_link": "docs.anthropic.com",
      "snippet_matched": []
    }
  ],
  "search_parameters": {
    "q": "anthropic",
    "gl": "us",
    "hl": "en",
    "page": 1,
    "engine": "brave"
  }
}
```

### Response fields

| Field | Type | Description |
|---|---|---|
| `organic_results[]` | object[] |  |
| `organic_results[].link` | string |  |
| `organic_results[].title` | string |  |
| `organic_results[].domain` | string |  |
| `organic_results[].snippet` | string |  |
| `organic_results[].position` | integer |  |
| `organic_results[].displayed_link` | string |  |
| `organic_results[].snippet_matched` | string[] |  |
| `search_parameters` | object |  |
| `search_parameters.q` | string |  |
| `search_parameters.gl` | string |  |
| `search_parameters.hl` | string |  |
| `search_parameters.page` | integer |  |
| `search_parameters.engine` | string |  |

## Code examples in other languages

Ready-to-run examples live in the [`examples/`](examples) folder. Each one reads your key from the `PILOTERR_API_KEY` environment variable.

| Language | Search |
|---|---|
| cURL (Bash) | [`search`](examples/curl/search.sh) |
| PHP | [`search`](examples/php/search.php) |
| C# | [`search`](examples/csharp/Search.cs) |
| Go | [`search`](examples/go/search.go) |
| Java | [`search`](examples/java/SearchExample.java) |
| Ruby | [`search`](examples/ruby/search.rb) |
| Node.js | [`search`](examples/nodejs/search.mjs) |

## Practical use cases

- **SEO rank tracking** — Track positions of your pages and competitors on target keywords.
- **SERP monitoring** — Watch features, snippets and new entrants on strategic queries.
- **LLM grounding** — Feed AI agents and RAG pipelines with fresh search results.
- **Ad intelligence** — See who advertises on your keywords and with which messages.
- **Market research** — Map competitors, news and trends at scale.

## Why Piloterr?

- **No infrastructure** — no proxies, headless browsers or CAPTCHA solving to run yourself.
- **Structured JSON** — stable, documented fields instead of brittle HTML selectors.
- **Pay per request** — transparent credit cost per endpoint, failed requests are retried for you.
- **One API key, hundreds of sources** — Brave plus many other e-commerce, search and company data endpoints in the [Piloterr library](https://www.piloterr.com/library).
- **Any language** — plain HTTPS + JSON, with examples for Python, TypeScript, cURL, PHP, C#, Go, Java, Ruby and Node.js.

## FAQ

### Is it legal to scrape Brave?

Collecting publicly available data is generally allowed in many jurisdictions, but rules depend on your country, the data you collect and how you use it. Avoid personal data unless you have a lawful basis, respect the website's terms where they apply, and get legal advice for your specific use case. This is not legal advice.

### Do I need proxies or a headless browser?

No. Piloterr handles rendering, proxy rotation and retries on its side. You only send an HTTPS request with your `x-api-key` header and receive JSON.

### How much does a request cost?

Credits per call: `/v2/brave/search` **1**. See the [Piloterr website](https://www.piloterr.com) for plans.

### What data does the Brave API return?

Each endpoint returns structured JSON: **Brave Search** — `organic_results[]`, `organic_results[].link`, `organic_results[].title`, `organic_results[].domain`, `organic_results[].snippet`, `organic_results[].position`.

### How do I paginate Brave results?

Use the `page` parameter and keep requesting while the response indicates more results.

### Can I target a specific country or language?

Yes, use `gl`, `hl` — see the parameter tables above for accepted values.

### Which languages are supported?

Any language that can make an HTTP request. This README shows Python and TypeScript; the [`examples/`](examples) folder contains cURL, PHP, C#, Go, Java, Ruby and Node.js.

## Related scrapers

- [Bing Scraper API](https://github.com/piloterr/bing-scraper)
- [Google Scraper API](https://github.com/piloterr/google-scraper)
- [Google Ads Transparency Scraper API](https://github.com/piloterr/googleadstransparency-scraper)
- [All Piloterr scrapers on GitHub](https://github.com/piloterr)

## Support

- 💬 Join the [Piloterr Discord](https://discord.gg/CFj3ph8aHU) to ask questions and share what you build.
- 📚 Browse the [API reference (OpenAPI)](https://app.piloterr.com/openapi) and the [Piloterr library](https://www.piloterr.com/library).
- 🔑 [Get your API key](https://app.piloterr.com).

---

<p align="center">
  <a href="https://www.piloterr.com">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="images/logo-dark.svg">
      <img src="images/logo.svg" alt="Piloterr" height="32">
    </picture>
  </a>
  <br>
  <sub>Built by <a href="https://www.piloterr.com">Piloterr</a> — web scraping APIs for e-commerce, search, real estate and more, one API key.</sub>
</p>

<sub>Brave is a trademark of its respective owner. This repository is not affiliated with or endorsed by Brave. Examples are released under the MIT license.</sub>
