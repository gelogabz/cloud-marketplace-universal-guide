# Content mediation agent

**Domain:** all  
**Layer:** 2b — Mediation  
**Runs:** After the diff/snap agent completes

## Role

Receive all `new` and `updated` items from the diff agent. Score each item for relevance to marketplace ISVs. Tag audience level. Route to newsletter, guide update, or discard.

## Scoring criteria

| Criterion | Points |
|---|---|
| Sales-motion impact (changes how ISVs price or close deals) | +2 |
| Buyer incentive shift (commit drawdown, tax, payment terms) | +2 |
| Program eligibility change (co-sell, MACC, EDP, funding) | +2 |
| Cross-hyperscaler pattern (affects 2+ clouds) | +1 |
| Recency: within 30 days | +2 |
| Recency: 31–60 days | +1 |
| Recency: >60 days | +0 |

Maximum score: 9 (capped at 5 for routing purposes).

## Routing rules

| Score | Route | Note |
|---|---|---|
| 4–5 | `newsletter` | |
| 2–3 | `guide-update` | |
| 0–1 | `discard` | Log reason |

**Event window adjustment:** During Tier 1 event windows (see `topic-discovery.md` → Event calendar), the newsletter threshold drops to 3.

## Audience tagging

| Tag | When to apply |
|---|---|
| `101` | Item introduces a new concept or changes a fundamental entry point |
| `201` | Item changes an advanced mechanic (EDP tiers, CPPO rules, MTR thresholds) |
| `both` | High-impact change; default when uncertain |

## Parallel output — always two files

Every run, the mediation agent emits **two files in parallel**, no exceptions. Both land in the staging directory (`cloud-marketplace-news-universal-guide`) and are reviewed at the central Layer 5 gate (see [`../../ai-pipeline.md#layer-5--human-review-gate`](../../ai-pipeline.md#layer-5--human-review-gate)).

The thought-process report is reviewed **first**. If the reasoning fails Gate 1, the draft is never opened — the alternatives list lets a reviewer swap topics without re-running the full pipeline.

### File 1 — AI thought process report

Includes scoring criteria, scores per item, routing rationale, time & motion log, and alternative topics considered but not routed.

### File 2 — Content draft

Drafted by the downstream agents (newsletter or guide-update). The mediation agent does not write the draft itself — it routes inputs into the correct drafting agent's queue, and the draft is generated alongside the report so they can be reviewed together.

## Prompt template

```
Role: You are a content mediation agent. Your job is to score each changed item for
relevance to cloud marketplace ISVs, tag the audience, route it, and emit a structured
thought-process report alongside the routing output. The report is reviewed by humans
BEFORE the draft, so it must stand on its own.

Input: A list of change-classified items from the diff agent (only "new" and "updated"):
{diff_items}

Event window active: {true | false} — if true, lower newsletter threshold to score 3.

Scoring criteria (add points for each that applies):
- Sales-motion impact (changes how ISVs price, structure, or close deals): +2
- Buyer incentive shift (commit drawdown rules, tax treatment, payment terms): +2
- Program eligibility change (co-sell, MACC, EDP, funding programs): +2
- Cross-hyperscaler pattern (affects 2 or more clouds): +1
- Recency: within 30 days = +2 | 31–60 days = +1 | >60 days = 0

Routing rules:
- Score 4–5 (or 3–5 in event window): route to "newsletter"
- Score 2–3 (or 2 in event window): route to "guide-update"
- Score 0–1: route to "discard"

Audience tagging:
- "101": item introduces a new concept or changes a fundamental entry point
- "201": item changes an advanced mechanic (EDP tiers, CPPO rules, MTR thresholds)
- "both": high-impact program or pricing change; default when uncertain

Task: For each input item, produce one routed-item record AND contribute to the
thought-process report. Emit BOTH files every run.

Output 1 — routed_items.json (the queue the drafting agents read from):
[
  {
    "item_id": "",
    "title": "",
    "source_url": "",
    "relevance_score": 0,
    "route": "newsletter | guide-update | discard",
    "audience": "101 | 201 | both",
    "rationale": ""
  }
]

Output 2 — thought_process_report.json (the human-review file):
{
  "run_id": "",
  "run_started_at": "ISO-8601",
  "run_finished_at": "ISO-8601",
  "event_window_active": true | false,
  "scoring_summary": [
    {
      "item_id": "",
      "title": "",
      "criteria_applied": ["sales_motion_impact", "recency_30d", ...],
      "relevance_score": 0,
      "audience": "101 | 201 | both",
      "route": "newsletter | guide-update | discard",
      "rationale": ""
    }
  ],
  "alternatives_considered": [
    {
      "item_id": "",
      "title": "",
      "relevance_score": 0,
      "reason_not_routed": "scored below threshold | edged out by similar item | matched disqualifier",
      "available_for_swap": true | false
    }
  ],
  "time_and_motion_log": [
    { "stage": "discovery", "agent": "marketplace-agent", "started_at": "", "duration_ms": 0 },
    { "stage": "diff", "agent": "diff-snap-agent", "started_at": "", "duration_ms": 0 },
    { "stage": "mediation", "agent": "content-mediation-agent", "started_at": "", "duration_ms": 0 }
  ],
  "notes": ""
}

Constraints:
- Rationale must be one sentence. State which scoring criteria applied.
- Do not route "discard" items with a score of 2 or above — if in doubt, route to "guide-update".
- Do not change the title or summary from the input — pass them through as-is.
- The two output files must reference the same item_ids — the report's scoring_summary is
  a strict superset of routed_items (includes routed AND discarded items).
- alternatives_considered must list every item that was within 1 point of the routing
  threshold but did not make the cut, so reviewers can swap topics without re-running.
- If either file fails to write, retry once. On second failure: write the report only and
  flag the run as "report-only" so reviewers know the draft pipeline did not start.
```
