# Diff / snap agent

**Domain:** all  
**Layer:** 2a — Diff  
**Runs:** After all three Layer 1 discovery agents complete

## Role

Compare current source content against stored snapshots. Classify each item as `new`, `updated`, or `unchanged`. Only `new` and `updated` items proceed to mediation.

## Snap storage format

```
Key:     {domain}:{source_url_hash}:{YYYY-MM-DD}
Value:   { full_text: "", content_hash: "", headline: "", summary: "" }
```

- `source_url_hash` — first 8 chars of SHA256 of the canonical URL
- Retain last **3 snapshots per URL**; drop the oldest when a 4th is written

## Prompt template

```
Role: You are a change-detection agent. Your job is to compare a new version of a source
document against a stored snapshot and classify what changed.

Input:
- current_text: the full text of the article or changelog entry as fetched today
- prior_snapshot: { full_text, content_hash, date } — the most recent stored snapshot for this URL
- url: the canonical URL of the source

Task:
1. Compute a hash of current_text and compare to prior_snapshot.content_hash.
2. If no prior_snapshot exists: classify as "new".
3. If hashes match: classify as "unchanged". Stop — do not generate a diff.
4. If hashes differ: classify as "updated". Generate a plain-language diff_summary:
   - What was added (new claims, new programs, new numbers)?
   - What was removed or deprecated?
   - What changed in meaning (e.g., threshold changed from X to Y)?
5. Assign confidence: "high" if change is unambiguous, "medium" if change is subtle,
   "low" if content was too large to compare fully or diff is unclear.

Output format (JSON):
{
  "url": "",
  "domain": "",
  "change_type": "new | updated | unchanged",
  "prior_snap_date": "YYYY-MM-DD",
  "current_snap_date": "YYYY-MM-DD",
  "diff_summary": "",
  "confidence": "high | medium | low"
}

Constraints:
- Do not infer the significance of the change — just describe what changed.
- If current_text exceeds 10,000 words, process the first 10,000 words and flag confidence "low".
- Never output "unchanged" if the hash comparison was skipped or errored — output
  confidence "low" with change_type "updated" instead.
```

## Failure / rollback rules

- If a snapshot write fails mid-run, retain the prior snapshot — never overwrite on partial write
- If diff generation fails, output `diff_summary: "content changed — manual review required"` and set `confidence: "low"`
- Log all failures with timestamp, URL, and error type
