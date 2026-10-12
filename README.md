# pm-setup

> **In plain terms:** If you're a small team, your project tracker is probably a pile of issues with
> no priorities, no deadlines, and no way to tell which three things matter this week. This gets an
> AI assistant to set up the filing system for you: a small set of priority and type labels,
> milestones tied to real dates, and then it walks your existing backlog with you and sorts it.
> It's the half hour spent labelling the shelves before the paperwork arrives, except the shelves
> are already overflowing and it does the sorting too.

## What am I looking at?

This is a **skill for AI coding agents** like Claude Code, Cursor, Codex, and Gemini CLI. A skill
is a set of written instructions the agent reads and follows.

Unlike most skills, this one **only runs when you ask for it**. It won't trigger on its own in the
middle of other work, because reorganising somebody's issue tracker is not something that should
happen as a side effect. You call it by name.

## The problem it fixes

Project management for a team of one to five people has a specific failure mode. Either there's no
structure at all, so everything is equally urgent and nothing gets finished, or somebody installs a
heavyweight process built for a company ten times the size and quietly abandons it within a month.

This is deliberately the small version: **four priorities, four types, a handful of milestones.**
Enough to answer "what am I doing this week", not enough to need maintaining.

## What it sets up

**Four priorities, each with a meaning you can actually apply:**

| Label | What it means |
|---|---|
| `P0` | Drop everything. Users are blocked. |
| `P1` | Fix this week. It degrades the experience. |
| `P2` | Next milestone. Important, not urgent. |
| `P3` | Backlog. No timeline, and that's fine. |

**Four types:** `bug`, `feature`, `operations` (meaning it affects a customer you already have,
which is the one most teams are missing), and `meeting-notes` (it came out of a conversation, so
you can find it again).

**A milestone shape that matches how small teams actually work:** a near-term goal tied to a real
date or event, a polish phase after it, a growth phase with no date, and a backlog. Not quarters.

## What you can ask it to do

| Ask for | What happens |
|---|---|
| `full` | First-time setup. Labels, then milestones, then triage. |
| `labels` | Creates any missing labels. It never deletes or recolours ones you already have. |
| `milestones` | Asks what you're working towards and creates them. |
| `triage` | Walks your open issues in batches of five to ten, suggests a priority and a milestone for each, and waits for you to confirm before touching anything. |
| `status` | A dashboard: milestone progress, anything at `P0`, and everything still untriaged. |
| `create-from-notes` | Paste in meeting notes or a list of actions. It drafts an issue for each with a suggested priority and milestone, shows you all the drafts, and creates only the ones you approve. |

It also carries a weekly rhythm you can ask about: check `P0` and bugs daily, pick three to five
issues from the current milestone each week highest-priority first, give new items from meetings an
issue rather than building them on the spot, and run `status` at the end of the week.

## The rules it holds itself to

- It never deletes or closes an issue without asking.
- It never changes a label you already have, including its colour.
- It shows you what it's about to do before any bulk change.

Those exist because the whole point is to run this on a tracker with real work already in it.

## Requirements

- An AI coding agent that supports the Agent Skills standard: Claude Code, Cursor (2.4+), Codex,
  Gemini CLI, or GitHub Copilot CLI.
- The [`gh`](https://cli.github.com) GitHub CLI, signed in. This skill works on GitHub issues
  specifically, and does everything through `gh`.

## Install

| Agent | Where skills live |
|-------|-------------------|
| Claude Code | `~/.claude/skills/` (your home folder, used by every project) |
| Codex | `~/.agents/skills/` |
| Gemini CLI | `~/.agents/skills/` |
| GitHub Copilot CLI | `~/.agents/skills/` |
| Cursor (2.4+) | `.cursor/skills/` **inside the project** you want to use it in |

```bash
# Claude Code
git clone https://github.com/Tetteh-Yaw-Precious/pm-setup.git \
  ~/.claude/skills/pm-setup

# Codex, Gemini CLI, or Copilot CLI
git clone https://github.com/Tetteh-Yaw-Precious/pm-setup.git \
  ~/.agents/skills/pm-setup
```

In Claude Code, run it with `/pm-setup`, optionally with a repo and an action:

```
/pm-setup                      # auto-detects the repo, shows status
/pm-setup owner/repo full      # full first-time setup
/pm-setup triage
```

## Companion skills

| Skill | How it relates |
|---|---|
| [`feature-boilerplate`](https://github.com/Tetteh-Yaw-Precious/feature-boilerplate) | Creates the milestone, epic and user-story issues for one feature. Run `pm-setup` first so the labels and priorities it assigns already exist. |
| [`feature-cycle`](https://github.com/Tetteh-Yaw-Precious/feature-cycle) | The build loop. One of its three non-negotiables is that every piece of work gets a tracker issue, which assumes a tracker set up like this. |

## License

MIT.
