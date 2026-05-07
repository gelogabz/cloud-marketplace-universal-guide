# Cloud market agent

**Domain:** cloud-market  
**Layer:** 1 — Discovery  
**Runs:** In parallel with marketplace agent and co-sell agent

## Role

Monitor broader cloud market for trends, funding rounds, competitor moves, and macro shifts that affect marketplace ISVs — not direct changelog entries, but signal.

## Sources

| Source | Signal type | URL | Frequency |
|---|---|---|---|
| Last Week in AWS | RSS | `https://www.lastweekinaws.com/feed/` | Weekly |
| TechCrunch Cloud | RSS | `https://techcrunch.com/tag/cloud/feed/` | Weekly |
| The Register | RSS | `https://www.theregister.com/cloud/feed/` | Weekly |
| AWS main blog | blog-scrape | `https://aws.amazon.com/blogs/aws/` | Event-triggered only |
| Azure main blog | blog-scrape | `https://azure.microsoft.com/en-us/blog/` | Event-triggered only |
| GCP main blog | blog-scrape | `https://cloud.google.com/blog/` | Event-triggered only |

## Keyword groups

See [`topic-discovery.md` → Keyword mapping → Cloud market / macro](../topic-discovery.md) for the full keyword list.

## Prompt template

```
Role: You are a cloud market intelligence agent. Your job is to monitor broad cloud industry
sources for trends, program economics, and macro signals relevant to marketplace ISVs — not
direct changelog entries, but signals that affect strategy.

Input: A list of articles fetched from the following sources:
{source_list}

Task:
1. Filter items using the keyword list below. Keep items that match at least one keyword.
2. This domain covers macro trends and signals, not direct changelog entries. Include items
   about ISV market dynamics, program economics, and cross-hyperscaler patterns even if they
   do not match a keyword exactly — use judgment for high-signal items.
3. For each kept item, extract: title, source_url, date (YYYY-MM-DD), a 2–3 sentence summary,
   keywords_matched (list), and hyperscalers_affected (list from: aws, azure, gcp, snowflake).
4. If an item is ambiguous, include it and flag confidence as "low".

Keywords: {cloud_market_keywords}
Disqualifier keywords (drop any item matching these): {disqualifier_keywords}

Output format (JSON):
{
  "domain": "cloud-market",
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
- Flag editorial/opinion pieces with confidence "medium" even if keyword-matched.
```

## Failure behavior

If this agent fails, its sources are queued for the next Monday scheduled run. Failure is logged and flagged.
