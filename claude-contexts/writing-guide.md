# Suger Intelligence — Writing Guide

> Owned by: Dia Mercado
> Version: v1 — working framework

---

## Status of this document

**What this guide covers:** voice and tone, writing principles, formatting rules, a provisional set of content types derived from observed hyperscaler content patterns, and persona framing guidance.

**What is still TBD:** Content types are not finalized. They reflect patterns observed from the automation's sources and will evolve as the team publishes more issues and learns what resonates. The newsletter section structure (section order, layout, card rules) is pending Daniela's UX Outline, which will inform a future iteration of this guide.

**How to use it:** The voice, tone, and persona framing sections are stable — use them as-is. The content type templates are starting points, not fixed rules. When a piece of content does not fit cleanly into one of the provisional types, write to the principles in Part 2 and flag it for review.

---

# Part 1 — Before you write

---

## The three output streams

Everything produced belongs to one of three streams. The stream determines the content type, the voice, and the purpose.

**Universal Guide** — A living knowledge base organized around a marketplace maturity curve (101 → 201). Covers the foundational and intermediate concepts a Suger customer needs to navigate hyperscaler partnerships. Mostly static, updated when rules or programs change. The anchor everything else points back to.

**Recency Feed** — Surfaces what changed since the last snapshot: new features, policy changes, enforcement deadlines, fee updates. Time-sensitive and sourced.

**Newsletter** — A monthly publication assembled from both streams. Roughly 50% recency updates, 50% evergreen pulls from the Universal Guide. Designed to be worth reading on its own and to drive readers back to the Universal Guide.

---

## Content types

These six types cover the range of content the automation surfaces. They are a working framework — not final.

| Content type    | Stream               | What it covers                                      |
| --------------- | -------------------- | --------------------------------------------------- |
| Action Required | Recency / Newsletter | A change with a deadline the reader must act on     |
| What's New      | Recency / Newsletter | A new capability the reader can now use             |
| Policy Update   | Recency / Newsletter | A rule, fee, or requirement that changed            |
| Concept         | Universal Guide      | Explains what something is and why it matters       |
| Reference       | Universal Guide      | States current rules, fees, or eligibility as fact  |
| Playbook        | Universal Guide      | Step-by-step guidance for one workflow, one persona |

---

## Know your reader

Before writing a single sentence, be clear on who is reading this and where they are on the learning curve. The same update lands completely differently depending on whether the reader has run co-sell programs for five years or just started their first cloud marketplace listing.

### The maturity curve

**101 — New to the space.** This reader has just started or is evaluating cloud marketplace. They may not know what terms like ACE, MACC, committed spend, or private offer mean. They need every acronym defined, every concept grounded in a real scenario, and every instruction explained from scratch.

**201 — Active and operational.** This reader has live listings and running co-sell programs. They know the basics. They need tactical depth, edge cases, and optimization guidance — not definitions of things they already know.

> When in doubt, write for the 101 reader. Practitioners can skim explanations they already know. Newcomers cannot fill gaps they do not know exist.

### The personas

| Persona                     | What they care about                                    | Their pain                                                 |
| --------------------------- | ------------------------------------------------------- | ---------------------------------------------------------- |
| Partnerships & Alliances    | Partner attach rate, co-sell velocity, indirect revenue | Co-selling is hard to operationalize manually              |
| Sales (AEs)                 | Closing deals fast, no new tools to learn               | Friction in the sales cycle slows everything down          |
| Finance & Deal Desk         | Quote-to-cash, revenue recognition, reconciliation      | Marketplaces add complex billing the ERP was not built for |
| Sales Operations & Biz Tech | Process consistency, CRM hygiene, integration stability | Marketplaces add fields and workflows that create sprawl   |

A piece of content may be relevant to multiple personas, but it should be written from the perspective of the one who needs to act on it most urgently. Persona framing is covered in detail in Part 2.

---

# Part 2 — How to write

---

## Voice and tone

### Lead with what the reader gets, not what happened

This is the most important rule. Every piece of content should be written from the reader's perspective — not from the perspective of the hyperscaler making the announcement or the feature being described.

The shift is from **update-first** to **reader-first**:

❌ Update-first: "AWS launched Express Private Offers, a new feature that allows sellers to predefine custom pricing in the marketplace."

✅ Reader-first: "Sales teams can now send custom-priced deals in minutes instead of days. AWS no longer requires a manual back-and-forth for every custom offer."

Ask yourself before every opening sentence: _"Am I announcing something, or am I telling this person what changed for them?"_

---

### Write for someone brilliant who is new to this

Assume your reader is smart, experienced in their own function, and completely new to cloud marketplace. They have never heard of ACE, MACC, private offers, or committed spend drawdown. They will not Google a term you use without explaining it — they will simply stop reading.

**Define every acronym on first use.** No exceptions.

❌ "MACC eligibility is required to draw from committed Azure spend."

✅ "MACC (Microsoft Azure Consumption Commitment) eligibility is required. MACC is a pre-committed budget that large enterprise buyers sign with Microsoft — and once your solution qualifies, buyers can use that existing budget to pay for your product, which speeds up procurement significantly."

**Bridge technical terms to plain language.** When introducing a mechanism or program, connect it to something the reader already understands before adding detail.

❌ "Marketplace Channel Private Offers (MCPO) now qualify for 100% commit drawdown."

✅ "When a buyer purchases your product through a channel partner on Google Cloud Marketplace, that deal can now count toward the buyer's committed Google Cloud spending budget — something that was not allowed before."

**Test the sentence.** Before finalizing any entry, read it as if you have never heard of cloud marketplace. If you would need to Google a word or phrase to understand what to do next, the entry needs more explanation.

---

### Keep it plain and direct

Use simple, everyday language. Suger's readers deal with enough complexity in cloud marketplaces — the writing should not add to it.

- Use plain verbs: "check," "update," "submit," "review" — not "leverage," "utilize," or "operationalize"
- One idea per sentence
- Short paragraphs — aim for 2–3 sentences before a line break
- Active voice always: say who does what

❌ Passive: "The offer is pushed to AWS Marketplace by Suger."
✅ Active: "Suger pushes the offer to AWS Marketplace."

❌ Passive: "The deadline must be met by all API integration teams."
✅ Active: "API integration teams must meet this deadline."

---

### Explain the "why" — but only when it matters

Context is only useful when it helps the reader make a decision, avoid a mistake, or understand a restriction. Skip it for obvious actions.

Add context when:

- A step is counter-intuitive or seems out of order
- An action is irreversible or high-stakes
- A restriction comes from a hyperscaler, not Suger
- A timing constraint will catch readers off guard (e.g., AWS's 90-day pricing lockout)

❌ Skips the "why" on a high-stakes step: "Set the usage dimension price to $0.01."

✅ Explains it: "Set the usage dimension price to $0.01. Once a listing is live, AWS prevents changes to pricing dimensions. This placeholder lets you bill overages later without creating an entirely new listing."

---

### No hype, no superlatives

Remove any language that sounds like marketing copy. Users want clear, trustworthy information. Hype reduces trust and creates unrealistic expectations.

Remove: "exciting," "powerful," "game-changing," "seamlessly," "revolutionary," "best-in-class," "cutting-edge," "innovative," "robust," "transformative"

Replace with a specific number, outcome, or concrete description.

❌ "This powerful new feature seamlessly transforms your co-sell workflow."

✅ "This change removes the 3–5 day delay between finalizing a deal and issuing a custom offer."

---

### Use consistent terminology

Use the same term for the same concept every time. Cloud providers use different names for similar concepts — mixing them creates confusion.

When introducing a hyperscaler-specific term, bridge it to the plain-language equivalent:

✅ "Check the active Entitlement in Suger (which appears as an 'Agreement' in your AWS console)."

Do not invent shorthand. If the official term is "AWS Customer Engagements (ACE)," do not write "AWS co-sell system" or "the ACE tool" in the same entry.

---

### Anticipate what the reader does not know

Go beyond the obvious. Address the edge cases, timing surprises, and platform restrictions that will trip readers up before they hit them.

- Flag restrictions that come from the hyperscaler, not Suger
- Set timing expectations: "AWS reviews typically take 5–7 business days"
- Warn about steps that can block the entire workflow if skipped
- Note common mistakes before the step where they happen, not after

---

## Formatting rules

### Use callouts for what cannot be missed

Callouts are visual blocks that pull critical information out of the flow. Use them sparingly — if everything is flagged, nothing stands out.

**📌 Note** — Clarifies expected platform behavior, normal delays, or provides extra context the reader might want but does not need to act on immediately.

**💡 Tip** — Suggests a best practice or shortcut that makes the workflow easier.

**⚠️ Warning** — Highlights a critical prerequisite, an irreversible action, or a step that will break the workflow if missed.

Limit to one callout per section. Place it before the step it relates to, not after.

---

### Use bold for things the reader needs to find

Bold is for scanning — it should point the reader's eye to buttons, tabs, field names, and key terms they need to identify on screen or in a decision.

✅ Bold for: UI buttons and tabs ("Click **New Private Offer**"), key terms on first definition, field names

❌ Do not bold for: general emphasis, company names, sentences, or anything you just want to make "stand out"

---

### Lists: numbered for sequence, bullets for options

- **Numbered lists**: When order matters — steps, prerequisites, ranked items
- **Bullets**: When order does not matter — options, characteristics, related items

Every item in a list should follow the same grammatical structure. If items start with verbs, all items start with verbs.

❌ Not parallel: "Go to the Offers section. Clicking New Private Offer. You should select a product. Now enter the pricing info."

✅ Parallel: "Go to the Offers section. Click **New Private Offer**. Select a product. Enter the pricing info."

---

### Paragraph and sentence length

- Aim for 2–3 sentences per paragraph. Break it before it becomes a wall.
- One idea per sentence.
- If a sentence needs a semicolon to hold two ideas together, split it into two sentences.

---

# Part 3 — Content type guide

---

## Action Required

### What it is

A change with a specific deadline. The reader must do something before a stated date or face a concrete consequence — an integration breaks, an eligibility lapses, access gets blocked.

### What triggers this type

- Enforcement dates for new requirements (MFA, API authentication, compliance)
- Sunset dates for features or programs
- Registration or enrollment deadlines
- Renewal requirement changes with a cutoff

### Which personas

Tag the persona whose workflow is most directly impacted. Action Required items most often affect **Biz Tech & Sales Ops** (system changes) or **Partnerships** (program eligibility). Tag all that apply, but write from the most affected persona's angle.

### Format rules

**Length:** 3–5 sentences.

**Required elements — all four:**

1. The deadline — exact date in the first sentence
2. The consequence of inaction — what breaks, lapses, or gets blocked
3. Who is affected — specific role or system, not "partners" in the abstract
4. What to do — concrete action before the reader has to scroll

**Template:**

```
[Cloud Provider] is [enforcing / requiring / sunsetting] [change] on [exact date].

After this date, [what breaks or gets blocked — be specific].

This affects [specific role or system].

To prepare: [numbered steps if more than one, or a single clear action].

Source: [Publication · Date · URL]
```

### Full example

**Source:** Microsoft Marketplace Partner Digest, April 2026

---

**Partner Center API integrations break without MFA — act before April 1, 2026**

Microsoft began enforcing multi-factor authentication (MFA) for all Partner Center API calls using app + user authentication, effective April 1, 2026. MFA is a security method that requires verifying identity with a second factor — like a code from an authentication app — in addition to a password.

Any API request made without a valid MFA token now returns a 401 error and is blocked. This affects Biz Tech and Sales Operations teams running automated integrations between their CRM and Partner Center — for example, workflows that sync marketplace offer status or share ACE (AWS Customer Engagement) co-sell data automatically. It does not affect integrations that use app-only authentication.

All existing Partner Center APIs already support MFA tokens, so this requires a configuration update to your authentication setup, not a full rebuild.

To prepare:

1. Audit all Partner Center API integrations that use app + user authentication
2. Update those integrations to send a valid MFA token with each request
3. Test in a sandbox environment before March 31

Source: Microsoft Marketplace Partner Digest · April 2026 · https://techcommunity.microsoft.com/blog/marketplace-blog/microsoft-marketplace-partner-digest--april-2026/4510353

---

### Persona framing

**Biz Tech & Sales Operations** — lead with what breaks in their system

> This affects any automated integration that connects your CRM to Partner Center using app + user authentication. Check your sync workflows, alert triggers, and any backend process that calls the Partner Center API — they will fail after April 1 without the MFA token update.

**Partnerships & Alliances** — lead with what breaks in their program workflows

> If your team uses automated workflows to share co-sell opportunities or sync partner referrals through Partner Center, those processes break after April 1 without an MFA update. Work with your Biz Tech team to confirm the fix is in place before the deadline.

**Sales AEs** — reassure quickly, point to the right owner

> This is a backend change to API security. If you use marketplace workflows directly in your CRM without a custom integration, you are likely unaffected. Check with your Biz Tech team if you are unsure.

---

## What's New

### What it is

A new feature, capability, or program option that now exists. No urgency — but the reader can do something they couldn't do before. The framing shifts from "here's what launched" to "here's what you can now do."

### What triggers this type

- New marketplace listing features or deal mechanics
- New co-sell tools or program enhancements
- New pricing or billing models becoming available
- New program benefits rolling out to eligible partners

### Which personas

Tag the persona whose workflow this most directly improves. What's New items often apply to **Sales** (deal mechanics), **Partnerships** (co-sell features), or **Biz Tech** (new integrations).

### Format rules

**Length:** 3–5 sentences.

**Required elements:**

1. What it replaces — what friction or limitation existed before
2. What is possible now — the new capability in plain language
3. Who can use it — specific role or eligibility condition
4. Optional: how to access it

**Before/now test:** If you cannot complete "Previously, [readers had to]...", the item may not be a What's New — it might be a Policy Update or Concept entry instead.

**Template:**

```
Previously, [what the reader had to do / what friction existed].

[Cloud Provider] launched [capability]. Now, [what the reader can do differently].

[Who can use this, and any eligibility conditions].

[Optional: how to access it.]

Source: [Publication · Date · URL]
```

### Full example

**Source:** AWS re:Invent 2025

---

**Send custom-priced deals in minutes — AWS Express Private Offers is now live**

Previously, issuing a custom-priced deal on AWS Marketplace required a multi-step process: a seller built the offer manually, sent it to the buyer through a separate portal link, and any change to terms restarted the cycle. For high-volume sales teams, this was consistently one of the biggest delays between agreeing on a deal and getting a formal offer in front of the buyer.

AWS launched Express Private Offers at re:Invent 2025. Sellers can now predefine approved pricing structures — including discounts, custom terms, and payment schedules — that the marketplace automatically surfaces to matching buyers. A deal that used to take 3–5 days to issue can now be sent in under an hour.

This is available to any seller with an active AWS Marketplace seller account. Access it in Seller Central under **Private Offers → Express Offers**.

Source: Labra re:Invent 2025 Recap · December 10, 2025 · https://labra.io/aws-reinvent-2025-recap/

---

### Persona framing

**Sales AEs** — lead with the deal delay it removes

> If you have a buyer ready to commit but waiting on a formal custom offer, that delay is gone. AWS can now issue pre-approved custom-priced deals automatically — no more waiting days for seller ops to build and send the offer manually.

**Partnerships & Alliances** — lead with channel impact

> Express Private Offers also works for channel-originated deals, not just direct ones. This reduces the manual effort of building a custom offer for each partner transaction.

**Sales Operations** — lead with the process change

> Express Private Offers automates the offer issuance step. If your current approval workflow includes a manual gate before an offer is sent, review whether that gate still applies — or whether pre-approved pricing structures can satisfy compliance requirements without the manual step.

---

## Policy Update

### What it is

A rule, fee, requirement, or program mechanic that changed. May or may not require immediate action — but it changes how the reader should think about or operate something going forward.

### What triggers this type

- Fee structure changes (listing fees, transaction fees, disbursement terms)
- Program eligibility or requirement changes
- Co-sell policy updates (new requirements, threshold changes)
- Regional availability changes (new geographies, tax treatment)
- Partner program restructuring

### Which personas

Policy Updates have the broadest reach. Tag all that apply, but lead from the most operationally affected persona. Fee changes lead with **Finance**; program requirement changes lead with **Partnerships**; workflow changes lead with **Sales Ops**.

### Format rules

**Length:** 4–6 sentences.

**Required elements — all five:**

1. What changed — from old value to new value (both sides, always)
2. Effective date
3. Plain-language explanation of what this means in practice
4. Operational impact for the reader
5. Action required — or explicit confirmation that no action is needed

**Template:**

```
[Plain-language description of what changed and why it matters].

[Cloud Provider] changed [policy/fee/requirement]
from [old value] to [new value], effective [date].

For [persona]: [specific operational implication for their workflow].

[Action required — or: no action needed, but update your [models / records / process].]

Source: [Publication · Date · URL]
```

### Full example

**Source:** Google Cloud Blog, May 2025

---

**GCP channel partner deals now count toward committed cloud spend — effective June 9, 2025**

Large enterprise buyers often sign multi-year agreements with Google Cloud, committing to spend a set amount on Google Cloud services. Previously, if a buyer purchased software through a channel partner on the Google Cloud Marketplace, that purchase did not count toward their committed budget — which pushed many enterprise buyers toward direct purchases instead of going through a partner.

Google Cloud changed this effective June 9, 2025. Qualifying software deals sold through authorized channel partners (known as Marketplace Channel Private Offers, or MCPO) now count 100% toward the buyer's committed Google Cloud spending budget, up to a cap of 25% of their total commitment.

For Finance and Deal Desk teams: update revenue recognition models for GCP channel deals dated after June 2025 to reflect the new 100% drawdown eligibility. No new program enrollment is required — the change applies automatically to qualifying MCPO transactions. For Partnerships teams: this removes a common objection when selling through the channel. Buyers no longer lose their committed budget credit by routing through a partner.

Source: Google Cloud Blog · May 15, 2025 · https://cloud.google.com/blog/topics/partners/upgrades-to-google-cloud-marketplace-for-partners

---

### Persona framing

**Partnerships & Alliances** — lead with the objection it removes

> Channel partners can now position GCP marketplace deals as commitment-eligible for enterprise buyers. Customers who previously pushed for direct transactions to protect their committed spend credit can now route through a partner without losing that benefit. Note the 25% cap — this works best for buyers who still have headroom in their commitment.

**Finance & Deal Desk** — lead with the number change

> For GCP channel deals (MCPO transactions) dated after June 9, 2025, update committed spend drawdown from 0% to 100% of offer price, capped at 25% of the customer's total GCP commitment. Update any RevRec models, deal desk pricing frameworks, or financial projections that pre-date this change.

**Sales AEs** — lead with the buyer objection it resolves

> Enterprise buyers with committed GCP spend who have been hesitant to route through a channel partner now have one fewer reason to say no. They keep their committed spend credit either way. Bring this up when the "direct vs. partner" question comes up in a deal.

**Sales Operations** — lead with the reporting implication

> If your CRM segments GCP deals by channel vs. direct, or tracks committed spend drawdown as a deal attribute, review those configurations. Channel deals now carry drawdown eligibility — this changes how they should be categorized in financial reporting.

---

## Concept

### What it is

An evergreen educational entry for the Universal Guide. Explains what a term, mechanism, or program is and why it matters to the reader. Written for readers who are encountering this for the first time — 101 level.

### What triggers this type

- A program or mechanism that appears frequently in hyperscaler content but has no plain-language explanation in the Universal Guide yet
- A concept that customers commonly misunderstand (per CS feedback or support tickets)
- 101-level topics from the educational backlog

### Which personas

Written primarily for readers at the 101 maturity level. Tag the persona most likely to encounter this concept first. Write from the angle of the person who needs to understand it most urgently to do their job.

### Format rules

**Length:** 400–600 words.

**Required elements:**

1. Headline: names the concept AND the benefit — not just the topic
2. Opening scenario: a situation the reader recognizes from their own experience
3. Plain-language explanation: one clear paragraph, every term defined on first use
4. Concrete example: a number, timeline, or real step the reader can picture
5. Next action: something specific the reader can do this week

**Headline formula:**

| ❌ Just the topic                | ✅ Concept + benefit                                                                                         |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| "MACC Overview"                  | "What is MACC, and how do you use a buyer's existing Azure budget to close deals faster?"                    |
| "ACE Opportunities Explained"    | "What is an ACE opportunity — and why AWS now requires them to stay in the co-sell program"                  |
| "Metered Billing on Marketplace" | "Metered vs. flat-rate pricing on marketplace: which model you choose affects how co-sell credit is counted" |

**Enforcement rule:** If the reader cannot act differently after reading the last paragraph, the entry fails. A piece that ends without a next action is a definition, not a Concept entry.

### Full example

---

**What is MACC — and how do you use a buyer's existing Azure budget to close deals faster?**

Picture this: you are in a late-stage enterprise deal. The budget conversation is going well, but procurement is slow. Then someone on the buying team mentions they have a large Microsoft Azure commitment they need to draw down before the year ends. If your product is not set up correctly, that existing budget cannot be used to purchase your solution — and you are back to a standard, slow procurement cycle.

MACC stands for Microsoft Azure Consumption Commitment. When large enterprise customers sign a multi-year cloud agreement with Microsoft, they commit to spend a set amount on Azure services. This creates a pre-approved budget pool that their procurement team is actively trying to use. When your product achieves Azure IP Co-sell Eligible status on the Microsoft Marketplace, purchases of your product can be drawn from that committed budget — which removes the need for a new budget approval, dramatically shortening the buying process.

To become MACC eligible, your offer needs to meet two requirements: it must be a transactable listing on the Microsoft Marketplace (meaning buyers can purchase it directly through the marketplace, not just request a quote), and you need to reach Azure IP Co-sell Eligible status in Microsoft Partner Center. The key threshold is generating $100,000 in Azure Consumed Revenue (ACR) — the total value of Azure services your customers use alongside your product — or $100,000 in Marketplace Billed Sales.

Partners who reach IP Co-sell Eligible status consistently report shorter enterprise deal cycles, because buyers with committed Azure spend actively look for ways to draw it down.

> 📌 **Note:** MACC eligibility only applies to transactable offers on the Microsoft Marketplace. If your listing is free, in preview, or available by request only, buyers cannot use their committed budget toward it.

**Next action:** Log into Microsoft Partner Center and check your current co-sell status under the Marketplace tab. If you are at Co-sell Ready but not yet IP Co-sell Eligible, the $100K ACR threshold is your next milestone. Every transactable marketplace deal moves you closer.

Source: Microsoft Learn · March 2026 · https://learn.microsoft.com/en-us/partner-center/referrals/co-sell-requirements

---

### Persona framing

**Sales AEs** — open with the deal scenario, close with a deal action

> Opening: You are in a late-stage deal and the buyer mentions they have committed Azure spend. Does your product qualify to draw from it?
> Close: Find out your MACC eligibility status today — it directly affects how you position the timeline on any enterprise Azure deal.

**Partnerships & Alliances** — open with partner enablement, close with a program action

> Opening: Your enterprise customers have committed Azure budgets. Can your partners use your product as a way to draw from them?
> Close: Share your IP Co-sell Eligible status with your partner team so they can lead with MACC in enterprise conversations.

**Finance & Deal Desk** — open with the procurement and billing difference, close with a RevRec note

> Opening: MACC-eligible deals have a different procurement profile — they draw from a pre-approved committed budget, which changes both deal velocity and how you recognize revenue.
> Close: Confirm with your Partner Center admin that your offer is configured as transactable. MACC drawdown only applies to transactable offers, not free or preview listings.

---

## Reference

### What it is

A factual, evergreen entry for the Universal Guide. States the current rules, fees, eligibility criteria, or requirements for a specific topic. Written for a reader who came here to look something up and confirm it is current — not to be educated, but to verify a fact.

### What triggers this type

- Any foundational marketplace rule, fee, or eligibility requirement that needs to be documented as source-of-truth
- When a policy changes and the Universal Guide section needs to be updated
- 101-level topics primarily about current rules rather than concepts

### Which personas

Reference entries are used by whoever needs to confirm a fact: **Finance** (fees, disbursement), **Sales Operations** (requirements, eligibility), **Partnerships** (partner tier rules). Write to be useful for all roles — Reference entries are not persona-specific in voice.

### Format rules

**Length:** As long as needed. No word count target.

**Voice:** Present tense. State what is true now. No editorializing. Exact numbers and dates only — never approximate. If you do not have the exact figure from an official source, use a `[VERIFY]` marker and flag for human review.

**Required elements:**

1. Descriptive heading (not benefit-driven)
2. Current state — exact figures, conditions, effective dates
3. One-sentence operational implication — what this requires in practice
4. Recent changes (if any) — from what to what, effective when. Keep the history.
5. Source + date last verified

> ⚠️ **Warning:** The most common failure mode for Reference entries is approximation. "Fees in the range of 3–5%" is a guess, not a Reference entry. If you cannot cite an exact number, do not publish — use `[VERIFY]` and flag it.

**Template:**

```
## [Topic heading — descriptive, not benefit-driven]

[Current state: exact numbers, conditions, no approximations.]

[Operational implication: one factual sentence about what this requires in practice.]

**Recent change** (if applicable):
Changed from [old value] to [new value], effective [date].

Source: [Official documentation URL]
Last verified: [Date]
```

### Full example

---

**GCP Marketplace Channel Private Offers — committed spend drawdown rules**

Qualifying software purchases made through authorized Google Cloud channel partners count 100% toward the buyer's committed Google Cloud spend, capped at 25% of the buyer's total Google Cloud commitment. This applies to purchases made through Marketplace Channel Private Offers (MCPO) — a specific deal type in which an authorized channel partner issues a private offer on behalf of an ISV.

Finance and Deal Desk teams modeling GCP channel deals should apply 100% drawdown eligibility, subject to the 25% cap. No special ISV enrollment is required — the policy applies automatically to qualifying MCPO transactions.

**Recent change:** Drawdown eligibility changed from ineligible (0%) to 100% of offer price, capped at 25% of total commitment, effective June 9, 2025. Previously, purchases through channel partners did not qualify for committed spend drawdown under any condition.

Source: Google Cloud Blog · https://cloud.google.com/blog/topics/partners/upgrades-to-google-cloud-marketplace-for-partners
Last verified: May 2025

---

## Playbook

### What it is

A step-by-step operational guide for one specific workflow, written for one specific persona at the 201 level. The reader already understands the concept — they need to know how to execute it correctly.

### What triggers this type

- 201-level topics from the educational backlog
- Workflows that appear in CS support tickets or onboarding friction
- Processes where ISVs commonly make the same mistakes

### Which personas

**One Playbook per persona per workflow.** A Sales AE and a Partnerships Manager performing the same process have different starting points, different tools, and different success criteria. Write them separately.

### Format rules

**Length:** As long as the workflow requires. Do not abbreviate steps for the sake of length.

**Required elements:**

1. Headline starting with "How to" — a task, not a topic
2. When to use this — the specific situation
3. Prerequisites — what needs to be in place before starting
4. Numbered steps — one action per step, specific enough to follow without guessing
5. Common mistakes — where things typically go wrong and how to avoid them
6. What good looks like — how to confirm the workflow completed correctly

> 💡 **Tip:** Common mistakes is the most underwritten section in most Playbooks. Think about what a support ticket for this workflow looks like — the mistake someone made is usually more instructive than a correctly-followed step.

**Template:**

```
## How to [specific action] — [persona]

**When to use this:** [The exact situation]

**Before you start:**
- [Prerequisite]

**Steps:**
1. [Specific action — include where in the platform to find it]
2. [Specific action]
...

**Common mistakes:**
- [Mistake and how to avoid it]

**What good looks like:**
[How to know you completed this correctly — specific status, confirmation, or visible outcome]
```

### Full example

---

**How to submit an ACE opportunity — Partnerships & Alliances**

ACE (AWS Customer Engagements) is the co-sell program AWS runs with its partners. When you submit an ACE opportunity, you are registering a customer deal with AWS so their field sales team can be involved — which unlocks co-sell support, increases visibility in AWS's system, and counts toward your ISV Accelerate program metrics (the program that governs your AWS partnership tier and benefits).

**When to use this:** You have an active deal with an AWS customer and want to bring AWS into the co-sell motion to increase the chance of closing.

**Before you start:**

- Your company is enrolled in AWS ISV Accelerate, or is at AWS Select tier or above with an active Marketplace listing
- You have the customer's AWS Account ID (a 12-digit number they can find in their AWS console under **Account Settings**)
- You have a rough deal summary: estimated close date, total contract value, and which AWS services the customer will use alongside your product

**Steps:**

1. Log into AWS Partner Central and go to **Co-Sell → Opportunities**
2. Click **Create Opportunity** and select **Co-sell with AWS** as the type
3. Enter the customer's AWS Account ID. This is the most critical field — without it, AWS cannot match your submission to a real customer in their system, and the opportunity will sit unassigned
4. Fill in the deal details: estimated close date, contract value, and the specific AWS services involved. List actual service names (for example: Amazon S3, Amazon RDS) rather than just "AWS" — this determines which AWS field team sees the opportunity
5. In the **Primary Need from AWS** field, select what you are actually asking for: technical support, an executive sponsor, customer introductions, or proposal review. The more specific you are, the faster the right person at AWS picks it up
6. Submit. The status shows **Submitted** immediately and moves to **Launched** once an AWS seller accepts it — typically within 5–7 business days

**Common mistakes:**

- **Missing the AWS Account ID.** This is the most common reason opportunities sit unassigned for weeks. If you do not have it, ask your customer contact directly — it takes them 30 seconds to find.
- **Selecting "Other" for AWS services.** AWS uses this field to route opportunities to the right team. "Other" goes to a general queue. Be specific even if the customer is still evaluating which services they will use.
- **Submitting before telling the customer.** ACE is a co-sell motion — AWS may reach out to the customer directly. Make sure the customer knows you are registering the opportunity before you submit. Surprises damage trust.
- **Setting the close date too far out.** AWS prioritizes deals closing within 90 days. If your deal is 6+ months away, wait until it is closer before submitting.

**What good looks like:**

Your opportunity shows status **Launched** in ACE Pipeline Manager — not just Submitted. You receive a Partner Central email confirming an AWS field seller has been assigned, and their name and contact information appear in the opportunity record. If the status stays at Submitted for more than 7 business days, check that the AWS Account ID is filled in correctly.

Source: AWS Partner Network · ACE Program Guide · https://aws.amazon.com/partners/programs/ace/

---

# Part 4 — Reference

---

## Newsletter voice

Structure and section order for the newsletter are defined in Daniela's UX Outline. This section covers voice only — how content sounds when it appears in the newsletter.

### Recency items — add one persona framing sentence

When a Recency Feed item (Action Required, What's New, or Policy Update) is pulled into the newsletter, the item is already written. Add one sentence of persona framing that connects it to the issue's primary audience.

```
[Recency Feed item — verbatim or lightly edited for length]

For [persona]: [one sentence connecting this to their specific workflow or concern.]
```

### Evergreen pulls — keep it brief and useful

When a Universal Guide entry is pulled into the newsletter, reframe it as 3–5 sentences — a "useful thing to know" with a point. It should read like something a knowledgeable colleague would mention in passing, not a formal excerpt from a reference document.

```
[The single most useful insight from the Guide entry — not the full entry, just the point]
[1–2 sentences of context: why this matters now, or how it connects to current conditions]
[One sentence: what this means for the reader's work]
```

---

## Quality checklist

Verify all three categories before finalizing. This checklist is used by both AI agents (self-review before returning output) and human reviewers (Gate 2 review before publication approval).

If anything fails, revise before returning or approving the output.

### Clarity — can the reader understand it?

- [ ] Does the headline name both the topic and the benefit (or task)?
- [ ] Does the first sentence lead with what the reader gets — not what the vendor did?
- [ ] Has every acronym been spelled out and defined on first use?
- [ ] Has every technical term been explained in plain language?
- [ ] Is each sentence doing one thing?

### Relevance — does it affect this reader?

- [ ] Is the content framed for the correct persona?
- [ ] Does each paragraph include at least one of: impact on role, operational change, recommended action, or risk/implication?
- [ ] Are all claims sourced with a URL and date?
- [ ] Does the entry meet the minimum completeness requirements for its content type?

### Actionability — can the reader do something?

- [ ] Is there a clear "so what" — not just a description of what happened?
- [ ] For Concept entries: can the reader act differently after reading the last paragraph?
- [ ] For Action Required entries: is the deadline and consequence of inaction explicitly stated?
- [ ] For Playbooks: does the entry include both Common Mistakes and What Good Looks Like?
- [ ] Have hype words (exciting, powerful, seamlessly, game-changing) been replaced with specifics?

### The final test

Read the entry as if you have never heard of cloud marketplace. If you would need to Google a term to understand what to do next, the entry is not ready.
