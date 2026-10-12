---
name: pm-setup
description: Set up project management framework on a GitHub repo: milestones, labels, and issue triage. Use when the user wants to organize issues, create milestones, set up PM structure, or triage a backlog.
disable-model-invocation: true
---

# PM Framework Setup

You are setting up a lightweight project management framework on a GitHub repository. This is designed for early-stage startups (1-5 devs) who need structure without overhead.

## Arguments

`$ARGUMENTS` may contain:
- A GitHub repo name (e.g., `owner/repo`): use it for all `gh` commands
- A specific action like `triage`, `milestones`, `labels`, `full`, or `status`
- If empty, auto-detect the repo from the current git directory and run `status`

## Actions

### `full` (default first-time setup)
Run all steps: labels, milestones, then triage.

### `labels`
Ensure these labels exist on the repo (create if missing, skip if exists):

**Priority labels:**
- `P0` (color: `B60205`): Drop everything, blocks users or pilot
- `P1` (color: `D93F0B`): Fix this week, degrades experience
- `P2` (color: `FBCA04`): Next milestone, important but not urgent
- `P3` (color: `0E8A16`): Backlog, no timeline

**Type labels:**
- `bug` (color: `D73A4A`): Something broken
- `feature` (color: `0075CA`): New capability
- `operations` (color: `E4E669`): Impacts current customers
- `meeting-notes` (color: `C5DEF5`): Came from a meeting

Do NOT delete or modify existing labels. Only create missing ones.

### `milestones`
Ask the user what milestones they need. Suggest a structure like:

1. **A near-term goal** (2-4 weeks out): tied to a real event/deadline
2. **A polish phase** (after the goal): feedback and fixes
3. **A growth phase** (no date): scaling, new customers
4. **Backlog** (no date): someday ideas

For each milestone, ask for: title, description, and due date (if any).
Create them via `gh api repos/{owner}/{repo}/milestones`.

### `triage`
List all open issues and help the user assign:
1. A priority label (P0-P3)
2. A milestone

Present issues in batches of 5-10. For each batch, suggest assignments based on:
- Issues with `bug` label → likely higher priority
- Issues tied to active customers → `operations` + higher priority
- Enhancement/UI polish → likely P2-P3
- Issues without any labels → need triage

Let the user confirm or adjust before applying. Apply via `gh issue edit`.

### `status`
Show a dashboard of the current state:
- Milestone progress (open/closed counts, % complete)
- P0 issues (if any; these need immediate attention)
- Issues without a milestone or priority label (untriaged)
- Summary table

### `create-from-notes`
When the user provides meeting notes or a list of action items:
1. Parse out actionable items
2. For each item, draft: title, description, suggested priority, suggested milestone
3. Present all drafts to the user for review
4. Create approved issues and assign labels + milestone

## Weekly Rhythm (reference for the user)

If the user asks about workflow or process:

1. **Daily**: Check `P0 + bug` issues, fix immediately
2. **Weekly**: Pick 3-5 issues from current milestone, highest priority first
3. **Meetings**: New items get an issue + label + milestone, not immediately built
4. **End of week**: Run `/pm-setup status` to review progress

## Rules

- Never delete or close issues without user confirmation
- Never change existing label colors or descriptions
- Always show the user what you're about to do before bulk operations
- Use `gh` CLI for all GitHub operations
- If no repo is detected, ask the user which repo to use
