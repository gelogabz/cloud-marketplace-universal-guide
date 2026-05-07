# Suger Intelligence — Writing Guideline

## Purpose
This guideline is used by AI agents generating content for the Suger Intelligence newsletter. Every piece of content produced must follow these rules to ensure it is relevant, readable, and actionable for the specific persona reading it.

---

## The one rule that overrides everything else
Never describe what something is without telling the reader what to do with that information. Every article, update, or section must answer the reader's implicit question: **"Why does this matter to me, and what should I do next?"**

---

## Tone & voice

- **Professional but plain.** Write like a smart colleague explaining something over Slack, not a press release.
- **Direct.** Lead with the most important thing. Don't bury the point in context.
- **Grounded.** No hype, no filler words ("exciting," "robust," "game-changing"). Use concrete numbers and specifics instead.
- **Empathetic.** Assume the reader is busy, has global coverage across multiple clouds, and is not an expert in everything. Meet them where they are.

### What to avoid
- Jargon on first use without explanation
- Passive voice
- Opening with "This article will cover..." or "In this piece, we explore..."
- Describing a feature without explaining its impact
- Generic intros that could apply to any article

---

## Content categories

### 1. What Changed
**Purpose:** Surface the most recent, confirmed updates from AWS, Azure, and GCP that affect how users operate on marketplace. This section is about recency — it must always be timely and specific.

**Rules:**
- Lead with the cloud provider and what changed in the first sentence
- Second sentence: what this means operationally for the reader
- Maximum 3 sentences per item
- Always include source and date
- Only include confirmed changes — never speculation

**Template:**
```
[Cloud Provider] [changed / launched / updated] [specific thing].
This means [operational impact].
[Optional: what to do next or what to watch for.]
```

**Good example:**
> AWS Marketplace launched Resale Authorization for EMEA. Channel partners can now resell ISV listings in EU and UK without needing a separate listing — cutting the time to activate a new regional partner from weeks to days.

**Bad example:**
> AWS has announced exciting new features for their marketplace platform that will enhance the partner ecosystem experience across regions.

---

### 2. Deep Dive (101 / 201 Curriculum)
**Purpose:** Teach foundational or intermediate concepts that help readers operate more effectively on cloud marketplace. This is the universal guide content — it should be evergreen and accurate.

**Rules:**
- Headline must name the concept AND the benefit, not just the topic
- Open with a real scenario the reader will recognize
- Explain the concept in plain language, then connect it to what the reader should do
- Close with a clear "what you should do" or "what to watch for"
- Target length: 400–600 words

**Template:**
```
[Scenario the reader recognizes]
↓
[What this concept is, explained plainly]
↓
[How it works in practice]
↓
[What the reader should do with this]
```

**Good headline:** "What is a co-sell motion — and why does it matter to your quota?"
**Bad headline:** "Introduction to Co-Selling with Hyperscalers"

---

### 3. Market Pulse
**Purpose:** Give the reader one meaningful data point or trend from the broader cloud market, connected back to their work on marketplace. Think of it as a "fun fact with a point."

**Rules:**
- One stat or finding as the headline
- 2–3 sentences of context (where it's from, what's driving it)
- One sentence connecting it back to the reader's specific work

**Template:**
```
[Specific stat or finding].
[Context: what's driving this, where it comes from].
For [persona], this means [direct connection to their work].
```

---

### 4. Latest News
**Purpose:** Cover notable news from hyperscalers, the partner ecosystem, or the cloud market that may affect how readers think about their marketplace strategy.

**Rules:**
- News-style lead: who, what, when
- Second paragraph: what this could mean for the reader — interpretive, not just a summary
- Flag whether this is time-sensitive or something to monitor over time

---

## Persona writing angles

When writing any content, identify the primary persona(s) it's most relevant to, then frame the content through their specific lens. The same update can be written four different ways for four different readers.

---

### Partnerships & Alliances
**What they care about:** Co-sell velocity, partner attach rate, indirect revenue, not losing deals because co-sell workflows fall apart manually.

**How to frame content:** Always connect back to pipeline impact and partner relationships. Use language around co-sell motion, partner attach, opportunity sharing, and activation timelines.

**Avoid:** Finance-heavy language, CRM admin detail, anything quota-focused.

**Example — AWS EMEA Resale Authorization launch:**
> Your channel partners in EU and UK can now resell your listing without you setting up a separate listing per country. That removes one of the most common blockers when activating new regional partners — fewer setup calls, faster time to first deal.

---

### Sales (AEs)
**What they care about:** Closing deals fast, not learning new tools, using committed cloud spend to unlock buyer budget that's already been allocated.

**How to frame content:** Make it about the deal. What helps them close faster, unblocks a stuck opportunity, or reduces friction in their existing workflow. Keep it short — AEs skim.

**Avoid:** Policy-heavy language, partnership strategy framing, anything that sounds like overhead.

**Example — Azure private offer expiry extended to 30 days:**
> If you've had buyers let offers expire because 14 days wasn't enough time for internal approvals, Azure just fixed that. The expiry window is now 30 days. Set your offer timelines accordingly and stop losing deals to clock-outs.

---

### Finance & Deal Desk
**What they care about:** Clean quote-to-cash, revenue recognition, accurate invoicing, knowing exactly when and how money moves from marketplace to their ERP.

**How to frame content:** Specifics matter most here. Use exact numbers, timelines, and reconciliation implications. Connect explicitly to their ERP/CRM workflow where possible.

**Avoid:** Co-sell strategy language, vague efficiency claims, anything without a concrete number or timeline.

**Example — GCP listing fee confirmed unchanged:**
> GCP's 3% transaction fee holds for Q2 2026. If you're building revenue recognition models or deal desk pricing for GCP deals this quarter, no rate adjustments needed. Disbursement timelines remain on the standard 30-day cycle.

---

### Sales Operations
**What they care about:** Process consistency, CRM hygiene, approval workflows that don't create sprawl or require constant manual intervention.

**How to frame content:** Frame everything as a process or system implication. What does this change mean for how their CRM is configured, how approvals are routed, or how data flows downstream?

**Avoid:** Sales-facing quota language, strategic framing, anything that doesn't have a process implication.

**Example — Azure private offer expiry extended to 30 days:**
> Azure's new 30-day offer expiry default gives your approval workflows more runway. Check any automated expiry alerts, CRM field logic, or notification rules tied to the old 14-day window — those may need to be updated to avoid false triggers.

---

### Biz Tech & CRM Admins
**What they care about:** Integration stability, system architecture, not introducing new fields or flows that clutter existing setups or create technical debt.

**How to frame content:** Technical specifics, integration implications, what changed at the system level and whether it requires any configuration action on their end.

**Avoid:** Sales strategy language, revenue framing, anything that doesn't have a direct system or integration implication.

**Example — Azure private offer expiry extended to 30 days:**
> Azure's offer expiry default changed from 14 to 30 days. If you have automation or field logic in your CRM tied to offer expiry dates — triggers, alerts, stage transitions — verify those rules still fire correctly under the new window.

---

## Tagging rules

Every piece of content must be tagged before publishing. These tags are used by the agent to categorize, filter, and route content to the correct persona feed.

| Tag type | Allowed values |
|---|---|
| `content_type` | `what-changed` · `deep-dive` · `market-pulse` · `news` |
| `maturity_level` | `incubation` · `101` · `201` · `301` |
| `primary_persona` | `partnerships` · `sales` · `finance` · `sales-ops` · `biz-tech` |
| `cloud` | `aws` · `azure` · `gcp` · `multi-cloud` · `general` |
| `recency_flag` | `time-sensitive` · `evergreen` |

---

## Quality checklist

Before finalizing any piece of content, verify:

- [ ] Does the headline name both the topic and the benefit?
- [ ] Does the first sentence lead with the most important thing?
- [ ] Is there a clear "so what" for the reader — not just a description?
- [ ] Is the content framed for the tagged persona, not a generic reader?
- [ ] Are all claims sourced and confirmed (no speculation in What Changed)?
- [ ] Is the length appropriate for the content type?
- [ ] Has all jargon been explained on first use?
- [ ] Are filler words ("exciting," "robust," "game-changing") removed?
