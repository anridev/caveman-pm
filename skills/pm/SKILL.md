---
name: caveman-pm
description: Compresses PM agent responses to cut token output 50-65% while preserving all structured data. Four modes control how deep compression goes — conversation only, through to Jira and Confluence artifacts.
---

# caveman-pm — PM Response Compression Skill

## What This Skill Does

Reduces output tokens from PM agents by applying PM-specific abbreviations and structural tightening. Does not change data — changes how much prose wraps the data.

Activate with `/slavic-mode [mode]`. Deactivate with `/slavic-mode off`.

---

## Modes

| Mode | Applies to |
|------|-----------|
| `light` | Conversational responses only |
| `jira` | Conversation + Jira artifacts (descriptions, AC, comments) |
| `wiki` | Conversation + Confluence artifacts (pages, release notes, PVGs) |
| `full` | Conversation + Jira + Confluence artifacts |

Default when no mode specified: `light`.

---

## Core Compression Rules (all modes)

### Response style
- No preamble. No "Sure!", "Great!", "Of course!". Start with the output.
- No trailing offers ("Let me know if you need anything else").
- Don't restate the command or narrate what you're about to do.
- One sentence max per bullet. Two nesting levels max.

### Standard abbreviations
| Expanded | Compressed |
|----------|-----------|
| In Progress | WIP |
| Done | ✓ |
| Blocked | ✗ |
| Acceptance Criteria | AC |
| Story Points | SP |
| Definition of Done | DoD |
| Definition of Ready | DoR |
| Needs PM Decision | Decide: |
| Recommended Actions | Actions: |

### Tables
- Drop columns that are entirely empty.
- Ticket summaries: 6 words max.
- Merge "Status" + "Notes" if Notes only annotates Status.

---

## Output Type Rules (all modes)

### Daily digest (`/daily`)
- Sprint table: Key | Title | Status | Owner — no other columns unless blocked data would be lost.
- Decisions: one-line bullets, no context prose.
- Action items: `[OWNER] — [action] by [date]`.

### Follow-up report (`/followup`)
- Stale/blocked tables: Key | Title | Owner | Days — no other columns.
- Recommended actions: `[OWNER] — [action]` only.

### User stories (`/story`)
- Keep As / I want / So that format (required for Jira).
- AC: `[context] → [action] → [result]` — one line per criterion.
- DoD: all 8 items, ≤5-word label each.
- SP: one line with estimate and rationale on same line.

### Roadmap items + PVGs (`/idea`, `/doc`)
- Drop intro paragraph. Start with structured content.
- Section headers stay. Body max 2 sentences per section.
- Background/context: 3 bullets max.

### Release notes (`/release-notes`)
- Drop narrative intro. Lead with categorized bullet list.
- Category headers stay (Feature, Enhancement, Bug Fix, Infrastructure).
- Each item: `[TICKET] — [what changed for the user]` — one line.

### Market intel (`/market-intel`)
- Competitor entries: 2 bullets max each.
- Skip "No significant updates" entries entirely.
- Combine MENA signals into one section if ≤3 items.

### RFO (`/rfo`)
- Never compress. Full format always required regardless of mode.

---

## Jira Artifact Rules (jira mode · full mode)

Apply to everything written to Jira:

- AC: `G: [context] / W: [action] / T: [result]` — one line per criterion.
- Story description: 2 sentences max.
- Epic description: 1 sentence + bullet list of outcomes.
- Comments: bullets only, no pleasantries, no "Hi team".
- UFRF2 summary: ≤10 words. Description: ≤3 bullets.

---

## Confluence Artifact Rules (wiki mode · full mode)

Apply to everything written to Confluence:

- No "This document describes..." or "This page covers..." intros.
- No standalone "Overview" section — merge content into first substantive section.
- PVG pages: Background max 3 bullets, Problem Statement max 2 sentences.
- Release note pages: bullet list first, no narrative preamble.
- Section intros removed — start each section with content.

---

## Auto-clarity Exceptions

Never compress in these situations:

- Before any Jira or Confluence write — always present full draft for PM review first.
- When PM signals confusion or repeats a question — normal prose until resolved.
- RFO output — always full format.

Resume compression after the exception resolves.
