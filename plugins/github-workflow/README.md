# github-workflow

GitHub development workflow skills for Claude:

- **create-issue** — turn a decision, task, bug, or open question into a well-formed GitHub issue.
- **work-on-issue** — pick up an issue, create a branch, and implement it following repo conventions.
- **create-pr** — push the branch and open a pull request linked to its issue.

None of these skills merge, force-push, or push to the default branch.

## Two access paths — by design

The skills pick their GitHub access by what's available in the environment they run in:

- **create-issue → `gh` CLI if present, otherwise the bundled `github-workflow` MCP connector.** In Claude Code it uses `gh`; in Cowork, whose sandbox has no `gh`, it uses the connector. It never uses any other GitHub connector — if a separate generic "GitHub" connector is also installed, it's ignored.
- **work-on-issue and create-pr → `git` and the `gh` CLI.** They run in Claude Code against a real local clone, where `git` and `gh` are the natural tools. They don't use the connector at all.

So the connector only matters when **create-issue** runs somewhere without `gh` (i.e. Cowork). Everywhere else, `gh` installed and authenticated is all you need.

## Setup for the connector (create-issue without `gh`, one-time)

Only needed where create-issue runs without `gh` — i.e. Cowork. This plugin **bundles the GitHub MCP connector** — see `.mcp.json`, which registers the GitHub server at `https://api.githubcopilot.com/mcp/`. Installing the plugin adds that connector automatically, so **you do not need to add a connector manually**. If you try, you'll get *"A server with this URL already exists"* — that's expected; cancel the dialog.

What you *do* need is a one-time authorization:

1. Open **Settings → Connectors**.
2. Find **github-workflow** in the list.
3. Click it and **Connect / Authorize** — this opens GitHub's OAuth sign-in in your browser.
4. Sign in and approve. The connector moves from *unauthenticated* to *connected*.

Once connected, create-issue's GitHub tools are available and act on the repos your GitHub account can access. If you authorize the connector while a Cowork session is already open, start a fresh session so it picks up the newly-connected connector.

## Setup for work-on-issue and create-pr

These run in Claude Code and only need the `gh` CLI installed and authenticated:

```bash
gh auth status   # already logged in?
gh auth login    # if not
```

They derive `owner/repo` from `git remote get-url origin`, so run them from inside the clone.

## Troubleshooting

- **"A server with this URL already exists"** — the connector is already installed by the plugin. Cancel the Add-connector dialog and authorize the existing one under Settings → Connectors instead.
- **create-issue says it has no GitHub access** — the `github-workflow` connector isn't authorized yet, or the session started before you authorized it. Complete the setup above, then start a fresh Cowork session.
- **create-issue keeps hitting a 403 / "not accessible by integration"** — it's likely using a *different* GitHub connector than `github-workflow`. This skill uses `github-workflow` only; make sure that connector is the one authorized and available.
- **work-on-issue / create-pr can't reach GitHub** — check `gh auth status`; run `gh auth login` if needed.
- **A skill says there's no git remote** — work-on-issue and create-pr derive `owner/repo` from `git remote get-url origin`; add the remote if it's missing. create-issue takes `owner/repo` from what you tell it, since there's no remote to read in the sandbox.
