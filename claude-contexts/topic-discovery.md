# Topic discovery — pipeline spec

This file owns the discovery layer of the Suger Cube pipeline. It covers sources, keyword mapping, the snap/diff mechanism, domain agent assignments, persona tagging, and the event calendar as a scheduling trigger.

**What this is not:** Edition planning templates, freshness rules for newsletter topics, and the educational backlog list live in `suger-newsletters-platform-prototype/claude-contexts/topic-discovery.md`. That file is edition-focused. This one is pipeline-focused.

---

## Source table (agent-readable)

Each row is a monitored source. The `signal_type` column tells the agent how to fetch: `RSS` (parse feed), `blog-scrape` (fetch HTML + extract articles), `API` (structured endpoint), `manual` (human-curated, not automated).

| Source | Domain | Agent | Frequency | Signal type | URL |
|---|---|---|---|---|---|
| AWS What's New | marketplace | Marketplace agent | Weekly | RSS | `https://aws.amazon.com/new/feed/` |
| AWS Marketplace Blog | marketplace | Marketplace agent | Weekly | blog-scrape | `https://aws.amazon.com/blogs/awsmarketplace/` |
| Azure Updates | marketplace | Marketplace agent | Weekly | RSS | `https://azurecomcdn.azureedge.net/en-us/updates/feed/` |
| Azure Marketplace Blog | marketplace | Marketplace agent | Weekly | blog-scrape | `https://techcommunity.microsoft.com/category/azure-marketplace` |
| GCP Release Notes | marketplace | Marketplace agent | Weekly | blog-scrape | `https://cloud.google.com/release-notes` |
| Snowflake Release Notes | marketplace | Marketplace agent | Weekly | blog-scrape | `https://docs.snowflake.com/en/release-notes` |
| AWS Partner Network Blog | co-sell | Co-sell agent | Weekly | blog-scrape | `https://aws.amazon.com/blogs/apn/` |
| Azure Partner Blog | co-sell | Co-sell agent | Weekly | blog-scrape | `https://partner.microsoft.com/en-us/blog` |
| GCP Partner Blog | co-sell | Co-sell agent | Weekly | blog-scrape | `https://cloud.google.com/blog/topics/partners` |
| AWS ISV Accelerate pages | co-sell | Co-sell agent | Bi-weekly | blog-scrape | `https://aws.amazon.com/partners/isv-accelerate/` |
| Last Week in AWS | cloud | Cloud market agent | Weekly | RSS | `https://www.lastweekinaws.com/feed/` |
| TechCrunch Cloud | cloud | Cloud market agent | Weekly | RSS | `https://techcrunch.com/tag/cloud/feed/` |
| The Register | cloud | Cloud market agent | Weekly | RSS | `https://www.theregister.com/cloud/feed/` |
| AWS main blog (events only) | cloud | Cloud market agent | Event-triggered | blog-scrape | `https://aws.amazon.com/blogs/aws/` |
| Azure main blog (events only) | cloud | Cloud market agent | Event-triggered | blog-scrape | `https://azure.microsoft.com/en-us/blog/` |
| GCP main blog (events only) | cloud | Cloud market agent | Event-triggered | blog-scrape | `https://cloud.google.com/blog/` |

---

## Keyword mapping per source

Discovery agents filter fetched content using these keyword groups. A source item must match at least one keyword from its assigned group to be kept. Disqualifier keywords are checked after — a match on any disqualifier drops the item.

### Marketplace mechanics keywords
Used by: Marketplace agent (all sources)

```
listing, private offer, SaaS listing, AMI listing, metered billing, flat-rate SaaS,
procurement API, CPPO, channel partner private offer, ISV contract, marketplace fee,
listing fee, co-sell attribution, commit drawdown, MACC eligibility, EDP drawdown,
Native App Framework, marketplace search ranking, marketplace entitlement,
marketplace deployment, marketplace trial, free trial, pay-as-you-go
```

### Co-sell / programs keywords
Used by: Co-sell agent (all sources)

```
ACE opportunity, co-sell eligible, co-sell motion, ISV Accelerate, ISV Accelerate tier,
partner tier, APN, APN tier, AWS Select Partner, AWS Advanced Partner, AWS Premier Partner,
Azure Solutions Partner, Azure ISV, GCP Premier Partner, GCP Ready, MACC, EDP,
committed spend, MPN, co-sell unlock, top tier partner, funding program, MDF,
MTR threshold, marketplace launched, marketplace eligible
```

### Cloud market / macro keywords
Used by: Cloud market agent (all sources)

```
cloud marketplace, ISV, hyperscaler, marketplace economics, AI marketplace,
agent listing, Bedrock Marketplace, Azure AI Foundry, GCP Vertex AI, Vertex Model Garden,
marketplace revenue, cloud commit, marketplace ROI, marketplace acceleration,
co-sell program, cloud program update, partner program change
```

### Disqualifier keywords (applied to all domains)
If an item matches any of these, drop it regardless of other matches:

```
internal tooling, engineering preview, developer preview, alpha release,
deprecation notice (without replacement), pure SDK update, CLI update,
documentation fix, typo correction, UI refresh (cosmetic), certificate rotation,
region expansion (no pricing or listing impact)
```

---

## Snap/diff mechanism spec

The diff/snap agent runs after all three discovery agents complete. It compares current source content against stored snapshots to classify what changed.

### Storage format

```
Key:     {domain}:{source_url_hash}:{YYYY-MM-DD}
Value:   { full_text: "", content_hash: "", headline: "", summary: "" }
```

- `source_url_hash` is a short hash (e.g., first 8 chars of SHA256) of the canonical URL
- `full_text` is the raw extracted text of the article or changelog entry
- `content_hash` is SHA256 of `full_text` (used for fast equality check)
- Retain last **3 snapshots per URL**; drop oldest when a 4th is written

### Comparison algorithm

1. Fetch `full_text` for the current item
2. Compute `content_hash`
3. Look up the most recent snapshot for this URL
4. If no prior snapshot: classify as `new`
5. If `content_hash` matches prior: classify as `unchanged` — stop, do not propagate
6. If `content_hash` differs: classify as `updated`
   - Generate a plain-language diff summary (semantic, not line-by-line): what was added, what was removed, what changed in meaning
7. Write new snapshot entry

### Rollback / failure handling

- If a snapshot write fails mid-run, retain the prior snapshot — never overwrite on partial write
- If diff generation fails (e.g., content too large), output `diff_summary: "content changed — manual review required"` and set `confidence: "low"`
- Log all failures with timestamp, URL, and error type

### Output schema per item

```json
{
  "url": "",
  "domain": "",
  "change_type": "new | updated | unchanged",
  "prior_snap_date": "YYYY-MM-DD",
  "current_snap_date": "YYYY-MM-DD",
  "diff_summary": "",
  "confidence": "high | medium | low"
}
```

Only `new` and `updated` items are passed to the Content Mediation agent. `unchanged` items are logged and dropped.

---

## Domain agent assignments

Each agent owns a specific domain and source set. If an agent fails, its sources are queued for the next scheduled run — not reassigned to another agent.

| Agent | Domain | Sources owned | Failure behavior |
|---|---|---|---|
| Marketplace agent | marketplace | AWS What's New, AWS Marketplace Blog, Azure Updates, Azure Marketplace Blog, GCP Release Notes, Snowflake Release Notes | Queue all sources for next Monday run; log as missed; alert human |
| Co-sell agent | co-sell | AWS Partner Network Blog, Azure Partner Blog, GCP Partner Blog, AWS ISV Accelerate pages | Same |
| Cloud market agent | cloud | Last Week in AWS, TechCrunch Cloud, The Register, main cloud blogs (event-triggered) | Same |

**Why no cross-agent fallback:** Each agent's prompt is domain-scoped. Cross-assigning would require reprompting and risks contaminating relevance scoring with off-domain context.

---

## Persona tagging rules

Applied by the Content Mediation agent to each routed item. Determines which audience the item is relevant to.

| Tag | Label | Audience | Content characteristics |
|---|---|---|---|
| `101` | Newcomer | New to cloud marketplace (< 1 yr active) | Explains what/why, defines jargon, covers first-time setup steps, foundational program mechanics |
| `201` | Practitioner | Active listings, running co-sell programs | Assumes marketplace baseline, covers optimization, edge cases, advanced program mechanics, multi-cloud nuance |
| `both` | Both | Mixed audience or broadly high-impact | Program eligibility changes, pricing structure changes, anything that overrides prior assumptions for all levels |

**Tagging rules for mediation agent:**

- Default to `both` when in doubt
- Tag `101` only if the item is introducing a concept that didn't exist before or changes a fundamental entry point
- Tag `201` only if the item is a change to an advanced mechanic (EDP tiers, MTR thresholds, CPPO channel rules, multi-cloud attribution)
- Never tag `201` if the item is the first public announcement of a feature — first announcements always get `both`

---

## Event calendar as planning trigger

Events are inputs to pipeline scheduling, not just awareness items. The pipeline responds differently during event windows.

### Event schedule

| Event | Typical timing | Domain focus |
|---|---|---|
| AWS re:Invent | Late Nov – early Dec | Marketplace + co-sell + cloud |
| Google Cloud Next | April | Marketplace + cloud |
| Microsoft Build | May | Marketplace + cloud |
| AWS Summit (North America) | June | Marketplace + co-sell |
| AWS Summit (regional series) | Mar – Jun | Marketplace (regional nuances) |
| AWS re:Inforce | June | Co-sell + compliance |
| Snowflake Summit | June | Marketplace (Snowflake-specific) |
| Microsoft Ignite | November | Marketplace + co-sell |

### Trigger behavior

**2 weeks before a Tier 1 event** (re:Invent, Cloud Next, Build, Ignite):
- Pre-stage an edition slot in the newsletter pipeline
- Flag cloud market agent to watch event-specific sources starting 1 week out

**Within 48 hours after a Tier 1 event:**
- Run an extra unscheduled discovery pass (all 3 agents)
- Content Mediation lowers routing threshold: items scoring 3 (instead of 4) are routed to newsletter

**Within 1 week after any event:**
- Run a standard scheduled pass if the weekly Monday run hasn't fired yet
- No threshold adjustment

**Tier 1 events:** re:Invent, Cloud Next, Build, Ignite  
**Tier 2 events:** AWS Summits, re:Inforce, Snowflake Summit (1-week lead time, no threshold adjustment)
