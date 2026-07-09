---
name: create-issue
description: Use when the user wants to record a decision, task, bug, or open question as a GitHub issue — "create an issue", "raise an issue", "track this on GitHub", "turn this decision into an issue".
---

# Create Issue

Turn a decision, task, bug, or open question into a well-formed GitHub issue on the repo's own remote. Create and read only — this skill never merges, never pushes, never deletes anything.

This skill can run in Claude Code or in Cowork, so pick the GitHub access path by what's actually available:

- **`gh` CLI, if it's installed and authenticated** (`gh auth status`) — the natural path in Claude Code against a local clone.
- **Otherwise, the bundled `github-workflow` MCP connector** (see `.mcp.json`, the GitHub server at `https://api.githubcopilot.com/mcp/`) — the path in Cowork, whose sandbox has no `gh`.

Use exactly one of those. Never use any other GitHub connector — if a separate generic "GitHub" connector is also installed, ignore it. Whichever path you take, this skill is create-and-read only: it never merges, never pushes, never deletes.

## Preflight

1. **Pick your GitHub access.** Check `gh auth status` first; if `gh` is installed and authenticated, use it. If not, use the `github-workflow` connector — its MCP tools (issue read/write, search) must be loaded. If neither is available, the connector just needs a one-time authorization: point the user to **Settings → Connectors → github-workflow → Connect** (GitHub OAuth) and stop until it's authorized; don't tell them to add a connector manually (it's already installed). See `README.md` for setup. Never fall back to a different generic "GitHub" connector.
2. **Find the repo.** Establish `owner/repo` for the target. With `gh`, derive it from `git remote get-url origin`. Without a remote (e.g. the Cowork sandbox), take it from what the user told you (e.g. "sixeyed/claude-at-work-scratchpad"). If it's genuinely unclear, ask — don't guess a repo name.

## Steps

1. Read the source material — the doc, register entry, or conversation the user pointed at. Base the issue on what it actually says, not on your summary of the topic.
2. Check for an existing issue covering the same item — `gh issue list --search ...` or the connector's issue search, scoped to `owner/repo`; if one exists, report it instead of creating a duplicate.
3. Draft the title in the **imperative** — the issue is a unit of work, so name the work:
   - ✅ `Decide multi-workspace tenancy model`
   - ✅ `Fix login lockout reset`
   - ❌ `Decision needed: multi-workspace tenancy` (a status, not an action)
4. Draft the body with this shape:
   - **Context** — where this came from, with a link to the source doc if there is one
   - **Options** — for decisions: each option with its trade-offs
   - **What needs deciding / doing** — a checklist of the open questions or acceptance criteria
5. Apply one label: `decision` for decisions, `bug` or `enhancement` otherwise. If the label doesn't exist in the repo, create it first.
6. Create the issue.

## Out of scope

Assignees, milestones, and projects — leave them for the user. If they ask for those in the same breath, create the issue first, then let them know those steps are theirs.

## Report

End by reporting the issue number and URL, and one line on what the body covers.
