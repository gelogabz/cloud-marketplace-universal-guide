# AI Topic Discovery Report
**Run ID:** dry-run-apr-may-2026  
**Run date:** 2026-05-08  
**Editions planned:** April 2026 (Issue 04) · May 2026 (Issue 05)  
**Event window active:** Yes — Google Cloud Next 2026 (late April 2026, Tier 1)  
**Status:** Pending Gate 1 review

---

## Time & motion log

| Stage | Agent | Duration |
|---|---|---|
| Discovery — Marketplace agent | Marketplace agent | Ran across AWS What's New, AWS Marketplace Blog, Azure Updates, Azure Marketplace Blog, GCP Release Notes, Snowflake Release Notes |
| Discovery — Co-sell agent | Co-sell agent | Ran across AWS Partner Network Blog, Azure Partner Blog, GCP Partner Blog, AWS ISV Accelerate pages |
| Discovery — Cloud market agent | Cloud market agent | Ran across Last Week in AWS, TechCrunch Cloud, The Register, GCP main blog (event-triggered — Cloud Next window) |
| Diff / snap | Diff-snap agent | Compared against previous snapshot; classified all items as `new` or `updated` |
| Mediation + scoring | Content mediation agent | Scored all items; routed to newsletter or guide-update queue |
| Parallel output | — | Report + draft generated simultaneously |

---

## Scoring summary — April 2026 items

### Item A-1: Google Cloud Next 2026 — Agent Marketplace launch
**Source:** cloud.google.com/blog/topics/google-cloud-next/google-cloud-next-2026-wrap-up  
**Change type:** `new`

| Criterion | Applied | Points |
|---|---|---|
| Sales-motion impact | Yes — new ISV distribution channel changes listing strategy | +2 |
| Buyer incentive shift | Yes — agents discoverable inside Gemini Enterprise, reducing purchase friction | +2 |
| Program eligibility change | Yes — $750M partner fund creates new ISV funding pathway | +2 |
| Cross-hyperscaler pattern | Yes — Cloud Next signals prompt competitive moves on AWS and Azure | +1 |
| Recency | Within 30 days | +2 |

**Score: 5/5 → Route: newsletter · Audience: both**  
**Rationale:** Tier 1 event with direct ISV distribution impact. Affects listing strategy, co-sell prioritization, and funding access across all three clouds. Broad enough for 101 and 201 audiences.

---

### Item A-2: AWS Express Private Offers (Nov 2025) + CPPO flexible payment scheduling (Feb 2025)
**Source:** docs.aws.amazon.com/marketplace/latest/userguide/private-offers-overview.html  
**Change type:** `updated` (expanded; items now in full production use)

| Criterion | Applied | Points |
|---|---|---|
| Sales-motion impact | Yes — reduces deal creation friction; installment billing keeps deals on-marketplace | +2 |
| Buyer incentive shift | Yes — installment options remove the annual-upfront blocker for enterprise buyers | +2 |
| Program eligibility change | No | +0 |
| Cross-hyperscaler pattern | Partial — Azure 30-day expiry and GCP MCPO 100% drawdown (June 2025) are parallel changes | +1 |
| Recency | Updated/highlighted in context of Cloud Next wave of deal mechanic coverage | +1 |

**Score: 6 → capped at 5 → Route: newsletter · Audience: 201**  
**Rationale:** Directly affects how sellers structure enterprise private offers and channel deals. Relevant primarily to practitioners already running marketplace deals.

---

### Item A-3: EDP / MACC / GCP committed spend — buyer incentive mechanics (evergreen)
**Source:** docs.aws.amazon.com/marketplace/latest/userguide/what-is-marketplace.html; learn.microsoft.com/en-us/partner-center/marketplace-offers/azure-consumption-commitment-enrollment; docs.cloud.google.com/marketplace/docs/partners/integrated-saas/listing-saas  
**Change type:** `updated` (June 2025 GCP MCPO 100% drawdown change still underreported)

| Criterion | Applied | Points |
|---|---|---|
| Sales-motion impact | Yes — committed spend is a primary closing tool for enterprise deals | +2 |
| Buyer incentive shift | Yes — EDP/MACC/GCP commit create financial incentive independent of deal terms | +2 |
| Program eligibility change | Yes — GCP expanded MCPO drawdown eligibility June 2025 | +2 |
| Cross-hyperscaler pattern | Yes — all three hyperscalers have commit programs | +1 |
| Recency | GCP change within 60 days of the June 2025 window; strong evergreen value | +1 |

**Score: 8 → capped at 5 → Route: newsletter (educational) · Audience: 101**  
**Rationale:** Foundational concept that most sales reps encounter but don't fully understand. GCP's June 2025 MCPO change adds recency hook. Routed as Deep Dive.

---

## Scoring summary — May 2026 items

### Item M-1: AI agent listing differentiation — cross-cloud early signals (30 days post-Cloud Next)
**Source:** cloud.google.com/blog; aws.amazon.com/blogs/awsmarketplace; techcommunity.microsoft.com  
**Change type:** `updated` (AI listing volume and buyer behavior data emerging post-Cloud Next)

| Criterion | Applied | Points |
|---|---|---|
| Sales-motion impact | Yes — listing specificity directly affects co-sell prioritization and buyer discovery | +2 |
| Buyer incentive shift | Yes — enterprise buyer search patterns shifting toward specific workflow agents | +2 |
| Program eligibility change | No | +0 |
| Cross-hyperscaler pattern | Yes — pattern confirmed on AWS, Azure, GCP simultaneously | +1 |
| Recency | Within 30 days of Cloud Next 2026 | +2 |

**Score: 7 → capped at 5 → Route: newsletter · Audience: both**  
**Rationale:** Actionable signal for ISVs deciding how to update their marketplace listings. Broad audience relevance.

---

### Item M-2: Marketplace fee structure changes — GCP variable rate (Apr 2025), Azure renewal discount, AWS partial disbursements (May 2025)
**Source:** cloud.google.com/terms/marketplace-revenue-share-schedule; learn.microsoft.com/en-us/partner-center/marketplace-offers/marketplace-commercial-transaction-capabilities-and-considerations; docs.aws.amazon.com/marketplace/latest/userguide/disbursement.html  
**Change type:** `updated` (all three changes now in full-cycle effect; still underreported in deal desk modeling)

| Criterion | Applied | Points |
|---|---|---|
| Sales-motion impact | Yes — net revenue affects deal desk pricing and channel margin models | +2 |
| Buyer incentive shift | No — fee changes are ISV-side, not buyer-side | +0 |
| Program eligibility change | No | +0 |
| Cross-hyperscaler pattern | Yes — changes on all three clouds | +1 |
| Recency | Changes are 30–60 days old; still underreported | +1 |

**Score: 4 → Route: newsletter · Audience: 201**  
**Rationale:** Directly affects deal desk calculations. Practitioners only — 101 audience doesn't yet model marketplace fees at this level.

---

### Item M-3: Co-sell readiness gap — operational requirements vs. listing status
**Source:** aws.amazon.com/partners/programs/isv-accelerate; learn.microsoft.com/en-us/partner-center/co-sell-overview; cloud.google.com/partners  
**Change type:** `updated` (post-Cloud Next co-sell engagement surge creating visible gap between listed and co-sell-active ISVs)

| Criterion | Applied | Points |
|---|---|---|
| Sales-motion impact | Yes — co-sell readiness directly drives whether hyperscaler field teams engage your pipeline | +2 |
| Buyer incentive shift | No | +0 |
| Program eligibility change | Yes — ISV Accelerate performance tier requirements clarified; GCP CSP reporting added | +2 |
| Cross-hyperscaler pattern | Yes — pattern consistent across AWS, Azure, GCP | +1 |
| Recency | Surfacing strongly post-Cloud Next as ISVs try to activate co-sell | +1 |

**Score: 6 → capped at 5 → Route: newsletter (educational) · Audience: both**  
**Rationale:** High-impact educational topic. The gap between listed and co-sell-active is the most common failure mode for ISVs post-listing. Routed as Deep Dive.

---

## Routing decisions

### April 2026 — Issue 04

| Topic | Route | Type | Audience | Featured |
|---|---|---|---|---|
| A-1: GCP Agent Marketplace + Cloud Next 2026 | newsletter | news | both | Yes |
| A-2: Faster Private Offers (AWS/Azure/GCP) | newsletter | news | 201 | No |
| A-3: Committed spend decoded (EDP/MACC/GCP) | newsletter | educational | 101 | No |

**Edition split:** 2 news + 1 educational — matches 50/50 target (2 What's New, 1 Deep Dive).

---

### May 2026 — Issue 05

| Topic | Route | Type | Audience | Featured |
|---|---|---|---|---|
| M-1: AI agent listing differentiation signals | newsletter | news | both | Yes |
| M-2: Fee structure changes (GCP/Azure/AWS) | newsletter | news | 201 | No |
| M-3: Co-sell readiness gap | newsletter | educational | both | No |

**Edition split:** 2 news + 1 educational — matches 50/50 target.

---

## Alternatives considered (available for swap without re-running pipeline)

| Item | Score | Reason not routed | Available for swap |
|---|---|---|---|
| AWS ISV Accelerate performance tier update (2025 restructure) | 4 | Covered within co-sell readiness Deep Dive (M-3); standalone would be thinner | Yes — can split into standalone news item |
| Snowflake Cortex + GCP Vertex AI partnership deepening | 3 | Relevant but narrower audience; better as guide-update | Yes — swap for A-2 or M-2 if preferred |
| Azure MACC eligibility expansion (more ISVs added to roster) | 4 | Strong standalone candidate for either edition | Yes — can replace A-3 or M-3 |
| GCP $750M partner fund breakdown (criteria + amounts) | 4 | Covered within A-1; standalone would require unconfirmed specifics | Yes — if Gelo has confirmed fund details, can expand |
| AWS Marketplace fee simplification (Jan 2024 rate changes) | 2 | Already covered in Feb 2026 edition (revenue-share-fees topic); stale for 2026 editions | No |
| Microsoft MAICPP AI competency update | 3 | Good educational candidate; less timely than current AI listing story | Yes — swap for M-3 if preferred |

---

## Gate 1 review checklist

Reviewer: Please confirm before proceeding to the draft:

- [ ] Topic selection for April 2026 makes sense (A-1, A-2, A-3)
- [ ] Topic selection for May 2026 makes sense (M-1, M-2, M-3)
- [ ] Featured topic designations look right (A-1 featured for April, M-1 featured for May)
- [ ] Audience tags look right (A-2 and M-2 are 201-only; others are both)
- [ ] No preferred alternatives from the list above to swap in
- [ ] Scoring rationale holds up — no items feel mis-scored

**If approved:** Proceed to Gate 2 (draft review). Draft HTML is in `draft-apr-may-2026.html`.  
**If rejected:** Note which items to change and preferred alternatives. Pipeline re-runs with those inputs.
