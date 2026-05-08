# Project: Cloud Marketplace News Universal Guide

## What this is

The **staging environment and engine room** for the Suger Cube automated content system. This is not a customer-facing project — it owns the agent instructions, the three skill files that drive every draft, the staging copies of every report and draft, and the universal guide structure.

The pipeline is built around a single seamless flow:

> **Topic Discovery → UX / Newsletter Outline → Writing Guide**

Every edition or guide update moves through those three skills in order. Each is owned by a different teammate.

| Stage | Skill file | Owner |
|---|---|---|
| 1. Topic Discovery | `claude-contexts/topic-discovery.md` | Gelo |
| 2. UX / Newsletter Outline | `claude-contexts/ux-outline.md` | Daniela |
| 3. Writing Guide | `claude-contexts/writing-guide.md` | Dia |

It feeds two downstream outputs:
- The **universal guide** (evergreen knowledge base, structure defined in `universal-guide-structure.md`)
- The **newsletter platform** (`suger-newsletters-platform-prototype/`) — drafts approved here get pushed to its `data/topics`

## File index

| File | Purpose |
|---|---|
| `ai-pipeline.md` | Pipeline orchestrator: 4-layer flow, agent roster, parallel report+draft output, staging rules, human review gates, links to all skill files and agent specs |
| `universal-guide-structure.md` | Guide outline: section list, persona mapping (101/201), maturity curve (incubation/activation/growth), last-verified logic |
| `claude-contexts/topic-discovery.md` | **Skill 1 (Gelo)** — source table, keyword mapping, snap/diff mechanism, domain agent assignments, persona tagging rules, event calendar as scheduling trigger |
| `claude-contexts/ux-outline.md` | **Skill 2 (Daniela)** — UX structure, layout rules, design principles, persona-aware section order, card pattern, navigation, export behavior |
| `claude-contexts/writing-guide.md` | **Skill 3 (Dia)** — tone, voice, content categories, persona writing angles, tagging rules, quality checklist |
| `claude-contexts/agents/` | Individual agent spec files (one per agent): role, prompt template, output format, failure behavior |
| `ai-thought-process-reports/` | Agent thought-process reports from pipeline runs — one `.md` file per run, documents scoring rationale, alternatives considered, and time/motion log. Retained for audit and pipeline tuning; not published. |
| `draft-newsletters/` | Staged newsletter drafts (HTML snippets with inline styles) awaiting human review. Approved drafts are pushed to `suger-newsletters-platform-prototype/data/topics`. Both approved and pending copies are retained here. |
| `samples/` | Email-ready HTML samples (pipeline run report, newsletter draft, CS newsletter example, writing-guide mockup) |
| `claude-contexts/features.md` | Feature inventory and changelog |

## Staging behavior

Drafts and reports live here through human review. Once approved:
- Newsletter drafts are pushed to `suger-newsletters-platform-prototype/data/topics`
- Guide updates are committed to the relevant section in `universal-guide-structure.md` (or its downstream rendered form) with `last_verified` stamped
- A copy of every draft (approved and pending) is retained here as an HTML snippet with inline styles

Nothing customer-facing is published from this directory.

## Relationship to other projects

- **Newsletter platform** (`suger-newsletters-platform-prototype/`) is a consumer of this pipeline. It keeps its own edition planning templates and rendered output. The three skill files in this project are the source of truth for how drafts are built before they land there.
- **Glossary** (`marketplaceglossary-prototype-gelobaring/`) is independent but shares workspace design system.

## CLAUDE.md maintenance

After any change to this project:
1. Update this file if structure, file index, or relationships change.
2. Update `claude-contexts/features.md` changelog with date + description.
3. If the change affects workspace-wide patterns, update `web-projects/CLAUDE.md`.
4. Copy updated files to `claude-md-files-backup/` using the paths below.

| File | Backup location |
|---|---|
| `cloud-marketplace-news-universal-guide/CLAUDE.md` | `../claude-md-files-backup/universal-guide-claude.md` |
| `cloud-marketplace-news-universal-guide/claude-contexts/features.md` | `../claude-md-files-backup/claude-contexts/universal-guide-features.md` |
| `cloud-marketplace-news-universal-guide/claude-contexts/topic-discovery.md` | `../claude-md-files-backup/claude-contexts/universal-guide-topic-discovery.md` |
| `cloud-marketplace-news-universal-guide/claude-contexts/ux-outline.md` | `../claude-md-files-backup/claude-contexts/universal-guide-ux-outline.md` |
| `cloud-marketplace-news-universal-guide/claude-contexts/writing-guide.md` | `../claude-md-files-backup/claude-contexts/universal-guide-writing-guide.md` |
| `cloud-marketplace-news-universal-guide/ai-pipeline.md` | `../claude-md-files-backup/universal-guide-ai-pipeline.md` |

**Do not update `~/.claude/CLAUDE.md` (global) unless explicitly asked.**
