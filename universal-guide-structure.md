# Universal guide structure

Defines the structure of the evergreen cloud marketplace knowledge base. Covers audience personas, maturity curve, section outline with owner-agent mapping, and last-verified / change trigger logic.

The universal guide is the source of truth for educational content. The newsletter agent pulls from it for the evergreen half of each edition. The guide update agent writes to it based on mediated pipeline items.

---

## Audience personas

| Persona | Label | Description | Entry point in guide |
|---|---|---|---|
| Newcomer | `101` | First or second marketplace listing. Has not run co-sell. Evaluating whether marketplace is the right channel. Needs to understand the fundamentals before optimizing. | Incubation sections |
| Practitioner | `201` | Active listings on 1–3 clouds. Running co-sell programs. Optimizing deal mechanics and commit drawdowns. Knows the basics; needs depth on edge cases and advanced programs. | Activation and growth sections |

**Tagging rule:** Every guide section is tagged with one of `101`, `201`, or `both`. Content written for `201` must not define terms a Practitioner already knows. Content written for `101` must not assume prior marketplace experience.

---

## Maturity curve

Three phases map to where an ISV is in their cloud marketplace journey. The universal guide is structured around this curve.

| Phase | What it means | Guide sections |
|---|---|---|
| **Incubation** | Evaluating or setting up first listing. Understanding the channel, pricing options, and eligibility requirements. | Fundamentals, pricing models, getting listed |
| **Activation** | Listed and transacting. Building co-sell motion, structuring private offers, enrolling in partner programs. | Private offers, co-sell enrollment, partner tiers, enterprise procurement |
| **Growth** | Optimizing committed spend drawdowns, expanding to multi-cloud, maximizing listing discoverability and deal velocity. | EDP/MACC optimization, multi-cloud strategy, listing ranking, FedRAMP, revenue recognition, deal mechanics |

---

## Section outline

All sections the guide will contain. Owner agent is always the Guide update agent — no section is written or edited by humans directly (humans approve changes, they don't author them).

`last_verified` is blank until the first human-approved update stamps it.

| Section | Maturity phase | Persona | Owner agent | Last verified | Change trigger |
|---|---|---|---|---|---|
| What is cloud marketplace? | Incubation | 101 | Guide update agent | — | Any marketplace listing or program change |
| Marketplace vs. direct sales | Incubation | 101 | Guide update agent | — | Program economics change or new comparison data |
| Pricing models: metered vs. flat-rate SaaS | Incubation | both | Guide update agent | — | Pricing mechanic announcement (any cloud) |
| Getting listed: AWS | Incubation | 101 | Guide update agent | — | AWS listing requirement or process change |
| Getting listed: Azure | Incubation | 101 | Guide update agent | — | Azure listing requirement or process change |
| Getting listed: GCP | Incubation | 101 | Guide update agent | — | GCP listing requirement or process change |
| Getting listed: Snowflake | Incubation | 101 | Guide update agent | — | Snowflake Native App or listing change |
| Private offers | Activation | 201 | Guide update agent | — | Private offer mechanic or CPPO rule change |
| Co-sell enrollment: AWS ISV Accelerate | Activation | 201 | Guide update agent | — | ISV Accelerate eligibility or tier change |
| Co-sell enrollment: Azure co-sell | Activation | 201 | Guide update agent | — | Azure co-sell or MPN rule change |
| Co-sell enrollment: GCP co-sell | Activation | 201 | Guide update agent | — | GCP co-sell eligibility or partner tier change |
| Partner tiers: APN, Azure Solutions Partner, GCP Premier | Activation | 201 | Guide update agent | — | Any partner tier threshold or benefit change |
| Enterprise procurement requirements | Activation | both | Guide update agent | — | Procurement rule, legal requirement, or security framework change |
| Professional services on marketplace | Activation | 201 | Guide update agent | — | Professional services listing mechanic change |
| EDP vs. MACC vs. GCP committed spend | Growth | 201 | Guide update agent | — | Program terms, thresholds, or drawdown rules change |
| Multi-cloud listing strategy | Growth | 201 | Guide update agent | — | Cross-cloud pattern detected by pipeline or program change on 2+ clouds |
| Listing optimization and search ranking | Growth | 201 | Guide update agent | — | Marketplace algorithm, ranking factor, or discoverability change |
| Snowflake + AWS dual listing | Growth | 201 | Guide update agent | — | Snowflake or AWS dual-listing mechanic change |
| Snowflake Native App monetization | Growth | 201 | Guide update agent | — | Snowflake Native App Framework or billing change |
| FedRAMP and regulated industry listings | Growth | 201 | Guide update agent | — | FedRAMP, HIPAA, or regulated industry listing requirement change |
| Revenue recognition (ASC 606) | Growth | 201 | Guide update agent | — | Accounting standard guidance change or new marketplace-specific ruling |
| End-of-quarter deal mechanics | Growth | 201 | Guide update agent | — | Deal mechanic change or strong pattern signal from pipeline |
| Agent listing economics (AI agents) | Growth | both | Guide update agent | — | AI agent listing, pricing, or marketplace mechanic change (Bedrock, Azure AI Foundry, Vertex) |

---

## Last-verified and change trigger logic

### What `last_verified` means

`last_verified` is the date a human reviewer approved a guide update for that section. It does not mean the section was manually audited — it means an agent-proposed change was reviewed and accepted.

- Sections with no `last_verified` date are **draft status** — they exist in the outline but have no approved content yet
- Draft sections are not shown to guide readers until they have at least one approved entry

### Staleness rule

If a section has not had an approved update in **12 months**, it is flagged for human review regardless of whether a change trigger fired. The pipeline generates a staleness alert at the next scheduled run.

### Change trigger behavior

When the Content Mediation agent routes an item to `guide-update`:
1. The Guide update agent maps the item to a section using the change trigger column above
2. The section status changes to `"pending review"`
3. The proposed change is queued at the human review gate
4. `last_verified` is only updated after the human approves — not when the draft is created

If a section receives a second change trigger while a draft is already pending review:
- The new item is added to the pending queue alongside the existing draft
- The human reviewer sees both changes together

### Guide update agent output (reference)

```json
{
  "guide_section": "",
  "change_type": "add | amend | flag-for-removal",
  "proposed_content": "",
  "rationale": "",
  "source_url": "",
  "trigger_item_id": "",
  "proposed_date": "YYYY-MM-DD",
  "confidence": "high | medium | low"
}
```

### New section candidates

If the Guide update agent cannot match an item to any existing section, it outputs:
```json
{ "guide_section": "new-section-candidate", "proposed_section_title": "", ... }
```

New section candidates are reviewed by Gelo. If approved, the section is added to this outline with its maturity phase, persona, and change trigger defined before any content is drafted.
