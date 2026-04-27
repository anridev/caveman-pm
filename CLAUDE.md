# CLAUDE.md — caveman-pm

## Project Overview

caveman-pm applies compression to PM agent outputs — cutting 50-65% of output tokens by tightening prose, abbreviating standard PM fields, and structuring data more densely. It does not use fragmented speech; it uses PM-specific abbreviations and table discipline.

Ships as a skill file (`skills/pm/SKILL.md`) consumed by any Claude Code PM project. The companion ClaudeCode command (`/slavic-mode`) activates it within a session.

---

## File Structure

### Source of truth — edit only these

| File | What it controls |
|------|-----------------|
| `skills/pm/SKILL.md` | All compression behavior: modes, output-type rules, Jira rules, Confluence rules, exceptions. Only file to edit for behavior changes. |

### Supporting files

| File | Purpose |
|------|---------|
| `README.md` | User-facing product description. Keep accurate with `SKILL.md`. |

---

## Compression Design Philosophy

PM outputs have consistent, predictable structure: tables with fixed columns, user stories with fixed sections, digests with fixed headers. Token waste comes from:

1. **Prose preamble** — "Sure! I'll now pull the sprint data and..." (0 information)
2. **Trailing pleasantries** — "Let me know if you'd like any changes!" (0 information)
3. **Redundant column data** — Notes columns that repeat Status, empty columns kept for symmetry
4. **Verbose AC/DoD** — "Given that the user is logged in, when the user clicks..." vs. `[logged in] → [click export] → [CSV downloads]`
5. **Confluence intros** — Every PVG and release note page starts with a paragraph describing itself

Rules target these five patterns specifically. Nothing else is compressed — data fidelity is the hard constraint.

---

## Modes

Four modes control how deep compression goes. See `skills/pm/SKILL.md` for full rule definitions.

| Mode | Scope |
|------|-------|
| `light` | Conversational responses only |
| `jira` | Conversation + Jira artifacts |
| `wiki` | Conversation + Confluence artifacts |
| `full` | Conversation + Jira + Confluence artifacts |

---

## What Is Never Compressed

- RFO documents (incident details must not be abbreviated — legal and customer risk)
- Any artifact presented for PM review before a write (full draft always shown first)
- Responses to confused or repeated questions (switch to normal prose until resolved)

These exceptions are defined in `SKILL.md` and must be preserved in any edit.

---

## Integration with ClaudeCode

The `/slavic-mode` command in ClaudeCode's `.claude/commands/slavic-mode.md` contains the full compression ruleset inline. It is the activation mechanism for PM projects using the ClaudeCode PM OS.

`skills/pm/SKILL.md` is the distributable, agent-agnostic version of the same ruleset — for use with other PM tooling or via `npx skills`.

Keep both in sync when changing compression behavior.

---

## Key Rules for Agents Working Here

- Edit `skills/pm/SKILL.md` for any behavior change. It is the source of truth.
- Never add preamble/pleasantry rules that conflict with auto-clarity exceptions — the exceptions override in all cases.
- Never invent token reduction numbers. Benchmark only from real runs if benchmarks are added.
- README must stay accurate with actual `SKILL.md` behavior. If a rule ships or is removed, update README.
