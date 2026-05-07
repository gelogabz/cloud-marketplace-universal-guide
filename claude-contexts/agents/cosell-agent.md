# Co-sell agent

**Domain:** co-sell  
**Layer:** 1 — Discovery  
**Runs:** In parallel with marketplace agent and cloud market agent

## Role

Monitor co-sell program updates across AWS, Azure, and GCP — eligibility changes, ACE mechanics, funding programs, partner tier updates.

## Sources

| Source | Signal type | URL |
|---|---|---|
| AWS Partner Network Blog | blog-scrape | `https://aws.amazon.com/blogs/apn/` |
| Azure Partner Blog | blog-scrape | `https://partner.microsoft.com/en-us/blog` |
| GCP Partner Blog | blog-scrape | `https://cloud.google.com/blog/topics/partners` |
| AWS ISV Accelerate pages | blog-scrape | `https://aws.amazon.com/partners/isv-accelerate/` |

## Keyword groups

See [`topic-discovery.md` → Keyword mapping → Co-sell / programs](../topic-discovery.md) for the full keyword list.

## Prompt template

```
Role: You are a co-sell intelligence agent monitoring partner program sources for changes
relevant to ISV co-sell eligibility, program mechanics, partner tiers, and funding.

Input: A list of articles or changelog entries fetched from the following sources:
{source_list}

Task:
1. Filter items using the keyword list below. Keep only items that match at least one keyword.
2. For each kept item, extract: title, source_url, date (YYYY-MM-DD), a 2–3 sentence summary,
   keywords_matched (list), and hyperscalers_affected (list from: aws, azure, gcp).
3. If an item is ambiguous, include it and flag confidence as "low".

Keywords: {cosell_keywords}
Disqualifier keywords (drop any item matching these): {disqualifier_keywords}

Output format (JSON):
{
  "domain": "co-sell",
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

If this agent fails, its 4 sources are queued for the next Monday scheduled run. Failure is logged and flagged to the human team.
