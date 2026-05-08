# Features: Cloud Marketplace News Universal Guide

## Feature inventory

| Feature | File | Description |
|---|---|---|
| AI pipeline orchestrator | `ai-pipeline.md` | 4-layer pipeline: discovery → mediation → parallel report+draft → staging+approval. Built around the seamless flow Topic Discovery → UX Outline → Writing Guide. Documents agent roster, handoffs, human review gates, update triggers, links to skill files and agent specs. |
| Topic Discovery skill (Gelo) | `claude-contexts/topic-discovery.md` | Source table with agent-readable signals, keyword mapping per source, snap/diff mechanism spec, domain agent assignments, persona tagging rules, event calendar as planning trigger. |
| UX / Newsletter Outline skill (Daniela) | `claude-contexts/ux-outline.md` | UX structure for the CS Newsletter: personas (Explorer/Tracker), core UX rules, page/section structure, navigation rules, persona-specific visibility rules, onboarding flow, export behavior. |
| Writing Guide skill (Dia) | `claude-contexts/writing-guide.md` | Tone & voice rules, four content category templates (What Changed, Deep Dive, Market Pulse, Latest News), five persona writing angles, tagging rules, quality checklist. |
| Universal guide structure | `universal-guide-structure.md` | Audience personas (101/201), maturity curve (incubation/activation/growth), section outline with owner-agent mapping, last-verified and change trigger logic. |
| Agent spec files | `claude-contexts/agents/` | Individual MD file per agent: role, sources, prompt template, output format, constraints, failure behavior. 7 agents total. |
| Sample outputs | `samples/` | Email-ready HTML samples: pipeline run report, newsletter draft, CS newsletter example, writing-guide mockup. |

## Changelog

| Date | Change |
|---|---|
| 2026-05-07 | Initial project setup. Created CLAUDE.md, features.md, topic-discovery.md, universal-guide-structure.md. Updated ai-pipeline.md with pipeline diagram, prompt specs, and explicit handoff table. |
| 2026-05-07 | Created claude-contexts/agents/ with 7 individual agent spec files. Replaced embedded prompts in ai-pipeline.md with links to agent files. Added writing guideline and newsletter outline placeholder sections to ai-pipeline.md and newsletter-agent.md. Created samples/ with two email-ready HTML samples (pipeline run + newsletter draft) using May 2026 real-content scenarios. |
| 2026-05-08 | Major revamp. Restructured ai-pipeline.md around the seamless flow Topic Discovery → UX Outline → Writing Guide. Added Layer 3 parallel output (thought-process report + content draft generated together) and Layer 4 staging directory rules. Brought Daniela's UX outline (`claude-contexts/ux-outline.md`) and Dia's writing guide (`claude-contexts/writing-guide.md`) in-tree from the workspace root, replacing the placeholders. Moved CS newsletter example and writing-guide mockup HTML into `samples/`. Removed obsolete `TODOFORLATER.md` (revamp items now implemented) and the workspace-root `ai-pipeline-revamped.md` (merged into `ai-pipeline.md`). |
| 2026-05-08 | Aligned agent specs with the revamped pipeline. `content-mediation-agent.md` now documents the parallel two-file output (routed_items.json + thought_process_report.json with scoring_summary, alternatives_considered, and time_and_motion_log). `guide-update-agent.md` relabeled from Layer 3a to Layer 3 — Drafting and its dedicated review gate removed in favor of the central Layer 5 gate. |
| 2026-05-09 | Added `ai-thought-process-reports/` and `draft-newsletters/` to the CLAUDE.md file index — both directories were active but undocumented. Clarified retention policy: all drafts (approved and pending) stay in `draft-newsletters/`; reports stay in `ai-thought-process-reports/` for audit and pipeline tuning. |
