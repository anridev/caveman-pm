# caveman-pm

**Cut 50-65% of PM agent output tokens without losing any structured data.**
Inspired by: JuliusBrussee/caveman


caveman-pm applies compression rules tuned for PM workflows — daily digests, user stories, release notes, PVGs, sprint follow-ups. It targets the token waste specific to PM outputs: prose preamble, trailing pleasantries, redundant table columns, and verbose Confluence intros.

It does not change data. It changes how much prose wraps the data.

---

## Before / After

### Daily digest sprint table

**Normal (62 tokens):**
```
| Ticket | Title | Status | Assignee | Priority | Last Updated | Notes |
|--------|-------|--------|----------|----------|--------------|-------|
| CB-412 | Allow agents to transfer live calls to another queue | In Progress | Berina | High | 2 days ago | Blocked on IVR config |
```

**Slavic mode (28 tokens):**
```
| Ticket | Title | Status | Owner |
|--------|-------|--------|-------|
| CB-412 | Allow agents to transfer calls | WIP ✗ | Berina |
```

---

### User story AC

**Normal (47 tokens per criterion):**
```
- Given that the agent is handling an active call, when the agent clicks the transfer button and selects a queue, then the call is routed to the selected queue without dropping.
```

**Slavic mode (18 tokens):**
```
- [active call] → [click transfer, select queue] → [call routed, not dropped]
```

---

### Confluence PVG intro

**Normal (38 tokens):**
```
## Overview
This document describes the Product Value Guide for the Call Transfer feature. It covers the background, problem statement, proposed solution, and success metrics.
```

**Slavic mode (0 tokens — removed entirely):**
The first substantive section starts immediately.

---

## Modes

| Mode | What gets compressed |
|------|---------------------|
| `light` | Conversational responses only |
| `jira` | Conversation + Jira artifacts (descriptions, AC, comments) |
| `wiki` | Conversation + Confluence artifacts (pages, release notes, PVGs) |
| `full` | Conversation + Jira + Confluence artifacts |

---

## Install (Claude Code — ClaudeCode PM OS)

The compression ruleset ships as `.claude/commands/slavic-mode.md` in the ClaudeCode project.

**Activate in session:**
```
/slavic-mode          # light mode (conversation only)
/slavic-mode full     # everything compressed
/slavic-mode jira     # conversation + Jira artifacts
/slavic-mode wiki     # conversation + Confluence artifacts
/slavic-mode off      # disable
```

---

## Install (other agents)

Drop `skills/pm/SKILL.md` into your agent context or rules directory and instruct the agent to activate with `/slavic-mode [mode]`.

---

## What is never compressed

- **RFO documents** — incident detail must not be abbreviated.
- **Pre-write drafts** — full draft always shown for PM review before any Jira/Confluence write.
- **Confused or repeated questions** — normal prose until the PM is unblocked.

---

## Compression targets by output type

| Output | Compression approach |
|--------|---------------------|
| Daily digest | Table columns dropped to 4; action items to one line |
| Follow-up report | Same table discipline; "Recommended Actions" → "Actions:" |
| User stories | AC condensed to one-line `→` format; DoD labels shortened |
| Release notes | Narrative intro removed; bullet list leads |
| Roadmap items | Intro dropped; body max 2 sentences per section |
| Market intel | 2 bullets max per competitor; empty entries skipped |
| Jira descriptions | 2 sentences max; AC to `G/W/T` single-line format |
| Confluence pages | Intros and "Overview" sections removed |

---

## Skill file

`skills/pm/SKILL.md` — the distributable, agent-agnostic compression ruleset. This is the source of truth for all compression behavior.
