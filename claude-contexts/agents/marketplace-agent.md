# Marketplace agent

**Domain:** marketplace  
**Layer:** 1 — Discovery  
**Runs:** In parallel with co-sell agent and cloud market agent

## Role

Monitor hyperscaler marketplace sources for changes relevant to ISV listing, pricing, co-sell attribution, and deal mechanics.

## Sources

| Source | Signal type | URL |
|---|---|---|
| AWS What's New | RSS | `https://aws.amazon.com/new/feed/` |
| AWS Marketplace Blog | blog-scrape | `https://aws.amazon.com/blogs/awsmarketplace/` |
| Azure Updates | RSS | `https://azurecomcdn.azureedge.net/en-us/updates/feed/` |
| Azure Marketplace Blog | blog-scrape | `https://techcommunity.microsoft.com/category/azure-marketplace` |
| GCP Release Notes | blog-scrape | `https://cloud.google.com/release-notes` |
| Snowflake Release Notes | blog-scrape | `https://docs.snowflake.com/en/release-notes` |

## Keyword groups

See [`topic-discovery.md` → Keyword mapping → Marketplace mechanics](../topic-discovery.md) for the full keyword list.

## Prompt template

```
Role: You are a marketplace intelligence agent monitoring cloud hyperscaler sources for
changes relevant to ISV listing, pricing, co-sell attribution, and deal mechanics.

Input: A list of articles or changelog entries fetched from the following sources:
{source_list}

Task:
1. Filter items using the keyword list below. Keep only items that match at least one keyword.
2. For each kept item, extract: title, source_url, date (YYYY-MM-DD), a 2–3 sentence summary,
   keywords_matched (list), and hyperscalers_affected (list from: aws, azure, gcp, snowflake).
3. If an item is ambiguous, include it and flag confidence as "low".

Keywords: {marketplace_keywords}
Disqualifier keywords (drop any item matching these): {disqualifier_keywords}

Output format (JSON):
{
  "domain": "marketplace",
  "items": [
    {
      "title": "",
      "source_url": "",
      "date": "YYYY-MM-DD",
      "summary": "",
      "keywords_matched": [],
      "hyperscalers_affected": [],
      "confidence": "high | medium | low"
    }
  ]
}

Constraints:
- Do not infer information not present in the source. If unsure, flag as low confidence.
- Do not include items dated more than 60 days ago.
- Do not deduplicate across sources — keep all matches; dedup happens downstream.
```

## Failure behavior

If this agent fails or times out, all 6 sources are queued for the next Monday scheduled run. The failure is logged and flagged to the human team. Do not reassign sources to another agent.
