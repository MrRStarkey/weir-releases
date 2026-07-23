# Parallel Programmer

Run multiple [Claude Code](https://docs.anthropic.com/en/docs/claude-code) tasks against one git repository **in parallel**. Each pool is an isolated workspace -- its own git worktree, branch, terminal, and file viewer -- where Claude works a ticket independently while the app tracks every task from assignment through PR merge.

![Windows](https://img.shields.io/badge/platform-Windows-blue)

---

## Download & Install

Go to the [latest release](../../releases/latest) and download the `.exe` installer.

> **Windows SmartScreen** may show an "unrecognized app" warning while the installer builds download reputation. The app is digitally signed (publisher: *Randall Starkey*). Click **More info > Run anyway**. The prompt fades as installs accumulate.

The app auto-updates after install -- you'll be notified when a new version is ready.

---

## What You Need

| Tool | Check | Required? |
|------|-------|-----------|
| **git** | `git --version` | Yes |
| **Claude Code CLI** | `claude --version` | Yes -- must be installed and signed in |
| **GitHub CLI (`gh`)** | `gh auth status` | Yes -- must be logged in |

The app checks these on first launch and shows install guidance if anything is missing. If the GitHub CLI isn't signed in, the app lets you authenticate directly from within -- no separate terminal needed.

---

## How It Works

1. **Open a repository** -- pick any local git repo folder.
2. **Add pools** -- each pool is a fresh, isolated checkout of your default branch. Add as many as you want to run in parallel.
3. **Assign a ticket** -- give it a ticket ID (becomes the branch name), a title, description, and acceptance criteria. Everything is handed to Claude.
4. **Launch** -- Claude starts working autonomously in the pool's terminal. With the default prompt, it pauses when work is ready so you can test locally before it pushes and opens a PR.
5. **Watch the lifecycle** -- the app advances each pool automatically:

| State | Meaning |
|-------|---------|
| IDLE | No ticket -- ready for assignment |
| ASSIGNED | Branch created, ticket captured, not launched yet |
| IN PROGRESS | Claude is working (or resumable) |
| PR CREATED | An open PR detected on the branch -- automatic |
| MERGED | PR merged -- pool ready to recycle |
| CONCLUDED | Marked done without a PR (investigation, spec, no change needed) |
| ON HOLD | Paused on an external blocker -- clock frozen, session preserved |

PR creation and merge detection happen automatically via GitHub. For work that ends without a PR, use **Conclude** so it's recorded as completed rather than abandoned.

---

## Key Features

### Parallel Workspaces
Multiple pools run concurrently, each in its own git worktree. Assign different tickets to different pools and they all progress independently.

### Autonomous Lifecycle Tracking
The app monitors GitHub for PR creation and merge events, advancing pool states automatically. No manual status updates needed.

### Per-Pool Workspace
Each pool has its own:
- **Claude terminal** -- the agent's live session, interactive at any time
- **Shell terminals** -- extra terminals in the pool's worktree for running tests, builds, or git
- **Files panel** -- changes as diffs against the base branch (including uncommitted work), with Monaco syntax highlighting

### Activity-Based Time Tracking
Two clocks per pool: **Elapsed** (wall-clock lead time) and **Active** (time someone was actually working). Activity is tracked per source -- you see separate indicators for your hands-on time vs Claude's working time.

### Cross-Repo Time Report
A single report aggregates every ticket across all your repositories: duration, active time, and the you/Claude split. Filter by date range or repo, drill into per-ticket cycle breakdowns, and export as CSV.

### Investigation & Spec Notes
Claude writes investigation and spec documents while working a ticket. Notes are stored outside your repo, kept per ticket, and browsable after the ticket is done.

### Hold & Resume
Pause a blocked ticket with **Hold** -- the clock freezes, the Claude session stops (preserving its conversation), and shell tabs stay open. Paste a GitHub PR link as the blocker and the app notifies you when that PR merges. **Resume** picks up exactly where you left off.

### Attention Indicators
Pool tabs show who needs you: pulsing **blue** when Claude finished and is waiting, pulsing **amber** when it's asking for permission, and a green check when a held ticket's blocker merged.

### Branch Re-Assignment
If a ticket's branch already exists (from a previous attempt or another pool), the app detects it and offers to resume the existing work or start fresh.

### Dashboard & Team Activity
At-a-glance pool overview, completed ticket history with timing stats, and a view of your team's open PRs and remote branches.

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| Ctrl+0...5 | Jump to Dashboard / pool 1-5 |
| Ctrl+Tab / Ctrl+Shift+Tab | Cycle inner / outer tabs |
| Ctrl+T / Ctrl+W | New / close terminal tab |
| Ctrl+Shift+A | Assign a ticket |
| Ctrl+Shift+L | Launch Claude |
| Ctrl+` | Focus the Claude tab |

---

## Where Your Data Lives

Everything the app creates lives **outside your repository** under `%LOCALAPPDATA%\Parallel Programmer\`. Your repo stays clean -- no files written inside it.

State is tracked per repository path: tickets, notes, pool assignments, and history all restore whenever you reopen the same folder.

---

## Requirements

- **Windows 10/11** (x64)
- **git** (any recent version)
- **Claude Code CLI** (installed and signed in)
- **GitHub CLI** (authenticated via `gh auth login`)

---

## Feedback

Found a bug or have an idea? Use **Report Issue** in the app (Ctrl+Shift+B) or open an issue in this repository.
