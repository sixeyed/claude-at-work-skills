# github-workflow

GitHub development workflow skills for Claude:

- **create-issue** — turn a decision, task, bug, or open question into a well-formed GitHub issue.
- **work-on-issue** — pick up an issue, create a branch, and implement it following repo conventions.
- **create-pr** — push the branch and open a pull request linked to its issue.

All skills act on the repo's own git remote. None of them merge, force-push, or push to the default branch.

## Setup (one-time)

This plugin **bundles the GitHub MCP connector** — see `.mcp.json`, which registers the GitHub server at `https://api.githubcopilot.com/mcp/`. Installing the plugin adds that connector automatically, so **you do not need to add a connector manually**. If you try, you'll get *"A server with this URL already exists"* — that's expected; cancel the dialog.

What you *do* need is a one-time authorization:

1. Open **Settings → Connectors**.
2. Find **github-workflow** in the list.
3. Click it and **Connect / Authorize** — this opens GitHub's OAuth sign-in in your browser.
4. Sign in and approve. The connector moves from *unauthenticated* to *connected*.

That's it. Once connected, the skills' GitHub tools are available and act on the repos your GitHub account can access.

### Do I need the `gh` CLI?

No. The bundled connector is the primary path and works in Cowork, Claude Code, and the desktop app. The `gh` CLI is only a fallback for environments where you'd rather use it and it's already installed and authenticated (`gh auth status`). In Cowork specifically, the shell runs in an isolated sandbox where `gh` is not installed — so the connector is the path to use.

## Troubleshooting

- **"A server with this URL already exists"** — the connector is already installed by the plugin. Cancel the Add-connector dialog and authorize the existing one under Settings → Connectors instead.
- **A skill says it has no GitHub access** — the connector isn't authorized yet. Complete the one-time setup above.
- **A skill says there's no git remote** — the skills derive `owner/repo` from `git remote get-url origin`. If your local clone has no `origin` (common for a fresh sandbox checkout), either add the remote, or tell the skill the `owner/repo` explicitly.
