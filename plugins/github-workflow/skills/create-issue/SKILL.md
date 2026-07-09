---
name: create-issue
description: Use when the user wants to record a decision, task, bug, or open question as a GitHub issue — "create an issue", "raise an issue", "track this on GitHub", "turn this decision into an issue".
---

# Create Issue

Turn a decision, task, bug, or open question into a well-formed GitHub issue on the repo's own remote. Create and read only — this skill never merges, never pushes, never deletes anything.

## Preflight

1. **Find the repo:** derive `owner/repo` from `git remote get-url origin`. No remote? Stop and tell the user to push the repo to GitHub first — don't guess a repo name.
2. **Find your GitHub access:** this plugin bundles the GitHub MCP connector (see `.mcp.json`), so prefer its tools first. If the connector's tools aren't available, it just needs a one-time authorization — point the user to **Settings → Connectors → github-workflow → Connect** (GitHub OAuth) and stop until it's authorized; don't tell them to add a connector manually (it's already installed). As a fallback, use the `gh` CLI if it's installed and authenticated (`gh auth status`) — but note `gh` is generally not available in a Cowork sandbox, so the connector is the path there. See `README.md` for the full setup steps.

## Steps

1. Read the source material — the doc, register entry, or conversation the user pointed at. Base the issue on what it actually says, not on your summary of the topic.
2. Check for an existing issue covering the same item (`gh issue list --search ...`); if one exists, report it instead of creating a duplicate.
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
