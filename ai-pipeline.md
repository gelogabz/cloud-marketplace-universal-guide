# AI pipeline — Suger Cube

Working reference for the automated content pipeline. Documents agent roles, job handoffs, prompts, update mechanisms, and human-in-the-loop checkpoints.

This pipeline is built around a single seamless flow:

> **Topic Discovery → UX / Newsletter Outline → Writing Guide**

Every edition or guide update moves through those three skills in order. Each skill is owned by a different teammate and lives as a linked file the agents load before drafting.

| Stage | Skill file | Owner |
|---|---|---|
| 1. Topic Discovery | [`claude-contexts/topic-discovery.md`](claude-contexts/topic-discovery.md) | Gelo |
| 2. UX / Newsletter Outline (design experience) | [`claude-contexts/ux-outline.md`](claude-contexts/ux-outline.md) | Daniela |
| 3. Writing Guide | [`claude-contexts/writing-guide.md`](claude-contexts/writing-guide.md) | Dia |

The pipeline doc below is the orchestrator. The three skill files are the substance.

---

## Pipeline in plain English

You kick it off. Three discovery agents go out and scout their lanes — one watching marketplace changes, one watching co-sell programs, one watching broader cloud market signals. They bring everything back. (See **Topic Discovery** for the full source list, keyword map, snap/diff mechanism, and event calendar.)

A diff agent compares what they found against last week's snapshot. It only cares about what actually *changed*.

A mediation agent takes those changes, scores them, and makes a call: is this urgent enough for the newsletter, or is it a quieter update to the universal guide? It also tags everything by audience — 101-level or 201-level content.

Here's where the parallel output kicks in. Before any draft is written, the mediation agent always produces two files simultaneously: a **thought process report** (how it scored things, why it made those calls, and a list of alternative topics it considered but didn't pick) and the **content draft** itself. You review the report first. If the reasoning looks wrong, you stop there — you don't even need to read the draft. If you want a different topic, the alternatives list is right there.

The content draft is built using the **UX/Newsletter Outline** for shape (section order, persona-aware layout, badge rules, export behavior) and the **Writing Guide** for voice (tone, persona angle, content category templates, tagging).

Both files land in the **staging directory** (`cloud-marketplace-news-universal-guide`). That directory is the engine room — it holds the agent instructions, writing guide, UX outline, discovery reports, and draft copies. Nothing customer-facing lives there.

Once a human approves, the draft gets pushed to the **newsletter site**, structured to match what's already in `data/topics`. Copies of everything — approved and pending — stay in staging as HTML snippets with inline styles.

Nothing ever publishes itself.

---

## Pipeline overview

Four layers:

1. **Discovery** — Three domain agents monitor hyperscaler sources in parallel (driven by Topic Discovery skill)
2. **Mediation** — A diff agent isolates what changed; a scoring agent decides where it goes
3. **Parallel output** — A thought process report and content draft are always generated together (draft uses UX Outline skill + Writing Guide skill)
4. **Staging + approval** — Everything lands in the staging directory; a human gate controls what gets pushed to the newsletter site

---

## Layer 1 — Discovery agents

Three domain-specific agents run in parallel. Each is scoped to a single domain so results stay focused and comparable. Sources, keywords, and frequency are all defined in the [Topic Discovery skill](claude-contexts/topic-discovery.md) — that file is the single source of truth for what the agents fetch.

### Agent 1: Marketplace agent

**Job:** Monitor hyperscaler marketplace sources for changes relevant to ISV listing, pricing, co-sell attribution, and deal mechanics.

**Sources monitored:**
- AWS What's New (`aws.amazon.com/new`)
- AWS Marketplace Blog (`aws.amazon.com/blogs/awsmarketplace`)
- Azure Updates (`azure.microsoft.com/en-us/updates`)
- Azure Marketplace Blog (`techcommunity.microsoft.com/category/azure-marketplace`)
- GCP Release Notes (`cloud.google.com/release-notes`)
- Snowflake Release Notes (`docs.snowflake.com/en/release-notes`)

**Search keywords:** marketplace listing, private offer, co-sell attribution, listing fee, procurement API, MACC eligibility, commit drawdown, Native App Framework, marketplace ISV

**Output format:**
```
{
  "domain": "marketplace",
  "items": [
    {
      "title": "",
      "source_url": "",
      "date": "YYYY-MM-DD",
      "summary": "",
      "keywords_matched": [],
      "hyperscalers_affected": []
    }
  ]
}
```

Full agent spec: [`claude-contexts/agents/marketplace-agent.md`](claude-contexts/agents/marketplace-agent.md)

---

### Agent 2: Co-sell agent

**Job:** Monitor co-sell program updates across AWS, Azure, and GCP — eligibility changes, ACE mechanics, funding programs, partner tier updates.

**Sources monitored:**
- AWS Partner Network Blog
- Azure Partner Blog (`partner.microsoft.com/en-us/blog`)
- GCP Partner Blog (`cloud.google.com/blog/topics/partners`)
- AWS ISV Accelerate program pages

**Search keywords:** ACE opportunity, co-sell eligible, ISV Accelerate, partner tier, APN, Azure Solutions Partner, GCP Premier, MACC, EDP, funding program, MTR threshold

**Note:** Co-sell program documentation is often updated without announcement. The diff/snap mechanism is especially important here — partner portal pages behind login walls can't be scraped automatically. Someone on the team needs to periodically check partner portals and feed changes in manually.

**Output format:** Same structure as marketplace agent, domain = `"co-sell"`.

Full agent spec: [`claude-contexts/agents/cosell-agent.md`](claude-contexts/agents/cosell-agent.md)

---

### Agent 3: Cloud market agent

**Job:** Monitor broader cloud market for trends, funding rounds, competitor moves, and macro shifts that affect marketplace ISVs — not direct changelog entries, but signal.

**Sources monitored:**
- Last Week in AWS (`lastweekinaws.com`)
- The Register (`theregister.com`)
- TechCrunch Cloud (`techcrunch.com`)
- Main AWS / Azure / GCP blogs (event periods only)

**Search keywords:** cloud marketplace, ISV, hyperscaler, Bedrock, Azure AI Foundry, GCP Vertex, agent marketplace, marketplace economics

**Output format:** Same structure, domain = `"cloud-market"`.

Full agent spec: [`claude-contexts/agents/cloud-market-agent.md`](claude-contexts/agents/cloud-market-agent.md)

---

## Layer 2a — Diff / snap agent

**Job:** Compare current source content against a stored snapshot. Flag what changed.

**Trigger:** Runs after discovery agents complete. Receives their raw output.

**Mechanism:**
1. For each item returned by discovery agents, fetch the full source page
2. Compare against the last stored snapshot of that URL (stored by date)
3. Classify the change:
   - `new` — URL not seen before
   - `updated` — content changed since last snap
   - `unchanged` — no meaningful delta
4. For `updated` items, generate a plain-language diff summary

**Snap storage:**
- Snapshot key: `{domain}:{source_url}:{YYYY-MM-DD}`
- Store the full text content and a hash for comparison
- Retain last 3 snaps per URL for rollback

**Output format:**
```
{
  "domain": "",
  "url": "",
  "change_type": "new | updated | unchanged",
  "diff_summary": "",
  "previous_snap_date": "YYYY-MM-DD",
  "current_snap_date": "YYYY-MM-DD"
}
```

**Human in the loop:** None at this stage. Diff output feeds directly to mediation.

Full agent spec: [`claude-contexts/agents/diff-snap-agent.md`](claude-contexts/agents/diff-snap-agent.md)

---

## Layer 2b — Content mediation agent

**Job:** Receive all diff-flagged items, score them, route them, and generate two output files in parallel.

**Input:** All `new` and `updated` items from the diff agent.

**Scoring logic (relevance score 1–5):**

| Criterion | Weight |
|---|---|
| Sales-motion impact (changes how ISVs price or close deals) | +2 |
| Buyer incentive shift (commit drawdown, tax, payment terms) | +2 |
| Program eligibility change (co-sell, MACC, EDP, funding) | +2 |
| Cross-hyperscaler pattern (affects 2+ clouds) | +1 |
| Recency (within 30 days = +2, 31–60 days = +1, >60 days = 0) | +2 |

**Routing rules:**

| Score | Action |
|---|---|
| 4–5 | Route to newsletter as `news` item |
| 2–3 | Route to universal guide update queue as `educational` candidate |
| 0–1 | Discard. Log reason. |

**Audience tagging:** Each routed item gets tagged:
- `101` — new to hyperscaler partnerships, needs foundational context
- `201` — familiar with the space, needs tactical depth
- `both` — relevant across all levels

Persona tagging rules in detail: [Topic Discovery → Persona tagging rules](claude-contexts/topic-discovery.md#persona-tagging-rules).

### Parallel output — always two files

Every time the mediation agent runs, it generates both files simultaneously. No exceptions.

**File 1 — AI topic discovery report:**
- Scoring criteria and scores per item
- Decision rationale (why each item was routed the way it was)
- Human-readable explanations of every choice
- Time and motion log (when each agent ran, how long each step took)
- Alternative topics considered but not routed — these are for human choice

**File 2 — Content draft:**
- The actual newsletter draft or guide update copy
- **Structure:** built from the [UX / Newsletter Outline skill](claude-contexts/ux-outline.md) — section order, persona-aware layout, card pattern, navigation rules, export behavior
- **Voice:** built from the [Writing Guide skill](claude-contexts/writing-guide.md) — tone, persona angles, content category templates, tagging rules, quality checklist

**Review order:** The report is reviewed first. If the reasoning looks off, the human rejects at that stage without reading the draft. If a different topic is preferred, the alternatives list in the report enables a swap without re-running the full pipeline.

**Output format:**
```
{
  "item_id": "",
  "title": "",
  "relevance_score": 0,
  "route": "newsletter | guide-update | discard",
  "audience": "101 | 201 | both",
  "rationale": ""
}
```

Full agent spec: [`claude-contexts/agents/content-mediation-agent.md`](claude-contexts/agents/content-mediation-agent.md)

---

## Layer 3 — Drafting agents (Newsletter + Guide update)

Both drafting agents are downstream consumers of mediation output. They differ only in destination.

### Newsletter agent

Drafts the customer-facing edition. Loads the **UX Outline** for structure and the **Writing Guide** for voice, then composes:

- 50% recency (mediated `news` items, within 60-day freshness window)
- 50% evergreen (one deep-dive section pulled from the universal guide, matched to the edition's audience level)

Output: draft HTML newsletter with edition metadata, source list, and `[VERIFY]` markers on anything uncertain.

Full agent spec: [`claude-contexts/agents/newsletter-agent.md`](claude-contexts/agents/newsletter-agent.md)

### Guide update agent

Proposes updates to the universal guide when mediation routes an item with `route: "guide-update"`. Maps the item to a section in [`universal-guide-structure.md`](universal-guide-structure.md) using the change-trigger column there.

Output: proposed `add | amend | flag-for-removal` change with rationale, source URL, and trigger item ID. Stamps `last_verified` only after human approval.

Full agent spec: [`claude-contexts/agents/guide-update-agent.md`](claude-contexts/agents/guide-update-agent.md)

---

## Layer 4 — Staging directory

**Directory:** `cloud-marketplace-news-universal-guide`

This is the engine room, not the product. It is not customer-facing.

### What lives here

- This pipeline doc (agent instructions)
- [`universal-guide-structure.md`](universal-guide-structure.md) — guide outline (Gelo)
- [`claude-contexts/topic-discovery.md`](claude-contexts/topic-discovery.md) — discovery skill (Gelo)
- [`claude-contexts/ux-outline.md`](claude-contexts/ux-outline.md) — UX/newsletter outline skill (Daniela)
- [`claude-contexts/writing-guide.md`](claude-contexts/writing-guide.md) — writing guide skill (Dia)
- [`claude-contexts/agents/`](claude-contexts/agents/) — individual agent specs
- [`samples/`](samples/) — email-ready HTML samples and writing-guide mockups
- AI topic discovery reports — one per pipeline run
- Draft copies — both pending and approved — as HTML snippets with inline styles

### What does not live here

- The published newsletter site (lives in `suger-newsletters-platform-prototype/`)
- Any customer-facing content

Drafts sit here during the human approval stage. Once approved, output is pushed to the newsletter site. Copies of all drafts — approved and pending — are retained here permanently as HTML snippets.

---

## Layer 5 — Human review gate

Nothing publishes itself. Every output passes through this gate.

### Gate 1 — Thought process report review

Before reading the draft, the reviewer checks the AI topic discovery report:
- Do the scores make sense?
- Is the rationale for each routing decision sound?
- Are there better options in the alternatives list?

If the report is rejected, the draft is not reviewed. The pipeline re-runs with human guidance on what to change.

If the report is approved, the reviewer proceeds to the draft.

### Gate 2 — Draft review

- Check: are all news items within the 60-day freshness window?
- Check: are sources cited and verifiable?
- Check: does the content match the audience persona for this edition?
- Check: does the structure match the [UX Outline skill](claude-contexts/ux-outline.md)?
- Check: does the voice match the [Writing Guide skill](claude-contexts/writing-guide.md) (quality checklist at the end of that file)?

**If approved:** Draft is pushed to the newsletter site. Output structure must match the schema in `data/topics`. A copy is kept in staging as an HTML snippet (inline styles only, no external dependencies).

**If rejected:** Draft is returned to the relevant drafting agent with reviewer notes. Report and draft copies remain in staging marked as `pending`.

**Who reviews:** All four team members (Jon, Gelo, Dia, Daniela).

**Turnaround target:** 48 hours.

---

## Full job handoff summary

```
You (human) kick it off
        |
        v
[Marketplace agent] --+
[Co-sell agent]       +---> [Diff / snap agent] ---> [Content mediation agent]
[Cloud market agent] --+                                      |
        ^                                                     | generates in parallel
        |                                     +--------------++--------------+
[Topic Discovery skill]                       v                              v
                                  [AI topic discovery report]        [Content draft]
                                              |                       ^         ^
                                              |                       |         |
                                              |              [UX Outline] [Writing Guide]
                                              |                  skill        skill
                                              +-------------+-----------------+
                                                            v
                                                 [Staging directory]
                                          (cloud-marketplace-news-universal-guide)
                                                            |
                                                  [Human review gate]
                                                            |
                                              +-------------+--------------+
                                              v                            v
                                    [Newsletter site]           [Universal guide]
                                    pushed to data/topics       versioned, timestamped
```

---

## Human-in-the-loop checkpoints summary

| Checkpoint | Trigger | Who reviews | Action |
|---|---|---|---|
| Report review | Mediation agent completes a run | All four team members | Approve → proceed to draft review; reject → re-run pipeline |
| Draft review | Report approved | All four team members | Approve → push to newsletter site or commit guide update; reject → return to drafting agent with notes |

**Rule:** The agents never self-publish. Every output that touches the newsletter site or the universal guide requires explicit human approval.

---

## Update mechanisms

**What triggers a pipeline run:**
- Scheduled: weekly (every Monday, covers last 7 days of source activity)
- Event-triggered: within 48 hours of a major hyperscaler event (re:Invent, Cloud Next, Build, Ignite, Summit) — see [Topic Discovery → Event calendar](claude-contexts/topic-discovery.md#event-calendar-as-planning-trigger)

**What triggers a guide update:**
- Any item with `route: "guide-update"` and relevance score ≥ 2
- Human-approved only

**What triggers a newsletter edition:**
- Manual trigger when an edition is planned
- Requires a completed edition plan (see newsletter project's `claude-contexts/topic-discovery.md`) as input

**Freshness enforcement:**
- News items expire at 60 days from trigger event
- If a queued news item ages past 60 days before an edition runs, it is automatically rerouted to `educational` or discarded

---

## Good to haves

### Reviewer notifications
On each human review gate trigger, automatically fire a Slack message or email to all four reviewers with a direct link to the staging draft. Simple webhook — low effort, high value for keeping reviews from stalling.

### Draft previews
HTML snippet copies in staging already work as previews. Adding a hosted preview link per draft closes the loop — reviewers can see a rendered version without needing to run anything locally.

### Forecasting dashboard
The AI topic discovery reports are structured data — scores, decisions, alternatives, timing. Compiled over time, they become a forecasting dataset: which topics recur, which domains are most active, what gets rejected and why. Build this last, after the pipeline is stable. The data will already be accumulating in staging from day one, so nothing needs to be rebuilt — just surfaced.

---

## Derivative documents

This pipeline doc is the orchestrator. The three skills that drive it are linked below; everything else (agent specs, guide structure) is supporting material.

| Document | Owner | Status | How it plugs in |
|---|---|---|---|
| Topic Discovery | Gelo | ✅ [`claude-contexts/topic-discovery.md`](claude-contexts/topic-discovery.md) | Feeds source list, keyword map, snap/diff mechanism, persona tagging, and event calendar to the discovery + mediation agents |
| UX / Newsletter Outline | Daniela | ✅ [`claude-contexts/ux-outline.md`](claude-contexts/ux-outline.md) | Used by the newsletter agent to structure section order, persona-aware layout, badges, navigation, and export behavior |
| Writing Guide | Dia | ✅ [`claude-contexts/writing-guide.md`](claude-contexts/writing-guide.md) | Used by the newsletter agent and guide update agent for tone, four persona writing angles, six content type templates (Action Required, What's New, Policy Update, Concept, Reference, Playbook), 101/201 maturity levels, and quality checklist |
| Universal guide structure | Gelo | ✅ [`universal-guide-structure.md`](universal-guide-structure.md) | Defines the sections that the guide update agent writes to |

---

## Explicit handoff table

| Handoff | From | To | Input | Output | Failure mode |
|---|---|---|---|---|---|
| Discovery → Diff | All 3 discovery agents | Diff/snap agent | Raw JSON items per domain | Change-classified items (`new`, `updated`, `unchanged`) | Log failed items; skip to next |
| Diff → Mediation | Diff/snap agent | Content mediation agent | `new` and `updated` items only | Scored items with route + audience tag | Log failed items; skip to next |
| Mediation → Report+Draft | Content mediation agent | Staging directory | Scored items + alternatives | Two files: thought-process report + content draft | Retry once; on second failure, write report only and flag |
| Mediation → Guide | Content mediation agent | Guide update agent | Items with `route: "guide-update"` | Draft section changes with `change_type` and `proposed_content` | Queue item for next run |
| Mediation → Newsletter | Content mediation agent | Newsletter agent | Items with `route: "newsletter"` | Draft edition with HTML + PDF output | Queue items for next edition |
| Guide agent → Human | Guide update agent | Human review gate | Proposed changes per section | Approve / reject decision per change | No timeout; changes stay queued |
| Newsletter agent → Human | Newsletter agent | Human review gate | Draft edition | Approve / reject decision | No timeout; draft stays in queue |

---

## Prompt specs

Each agent has its own spec file with the full prompt template, output format, constraints, and failure behavior. See the [`claude-contexts/agents/`](claude-contexts/agents/) directory.

| Agent | Layer | Spec file |
|---|---|---|
| Marketplace agent | 1 — Discovery | [`claude-contexts/agents/marketplace-agent.md`](claude-contexts/agents/marketplace-agent.md) |
| Co-sell agent | 1 — Discovery | [`claude-contexts/agents/cosell-agent.md`](claude-contexts/agents/cosell-agent.md) |
| Cloud market agent | 1 — Discovery | [`claude-contexts/agents/cloud-market-agent.md`](claude-contexts/agents/cloud-market-agent.md) |
| Diff / snap agent | 2a — Diff | [`claude-contexts/agents/diff-snap-agent.md`](claude-contexts/agents/diff-snap-agent.md) |
| Content mediation agent | 2b — Mediation | [`claude-contexts/agents/content-mediation-agent.md`](claude-contexts/agents/content-mediation-agent.md) |
| Guide update agent | 3 — Drafting | [`claude-contexts/agents/guide-update-agent.md`](claude-contexts/agents/guide-update-agent.md) |
| Newsletter agent | 3 — Drafting | [`claude-contexts/agents/newsletter-agent.md`](claude-contexts/agents/newsletter-agent.md) |
