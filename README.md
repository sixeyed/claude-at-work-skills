# Claude at Work — skills

A plugin marketplace for [Claude Code](https://code.claude.com). Claude is perfectly capable of working with GitHub out of the box, but every session does it slightly differently — issue titles in whatever format seems right at the time, branch names that don't link back to anything, and the occasional overenthusiastic push. The skills in this marketplace pin down one workflow so the output is consistent, and the risky steps always stay with you.

## What's in here

One plugin so far, with three skills that chain together into a full development loop:

- **create-issue** — turns a decision, task, or bug report into a well-formed GitHub issue on the repo's own remote. Imperative titles, a structured body, one label, and a duplicate check before anything gets created.
- **work-on-issue** — picks up an issue, creates a branch named `issue-NN-short-slug`, implements the change following the repo's conventions, and runs the tests. It ends at the final local commit — it never pushes.
- **create-pr** — pushes the branch and opens a pull request linked to its issue. Tests have to pass first; if they don't, nothing leaves your machine.

The split between the last two is deliberate. Committing locally and publishing a PR are different levels of commitment, and keeping them as separate skills means you take the publishing step on purpose, not because it felt like the obvious next thing.

None of the skills merge, force-push, or touch the default branch. Review and merge belong to humans.

## Installation

Add the marketplace and install the plugin from inside Claude Code:

```
/plugin marketplace add sixeyed/claude-at-work-skills
/plugin install github-workflow@claude-at-work-skills
```

The skills are picked up automatically, and the plugin bundles GitHub's official remote MCP server so it has GitHub access out of the box. The first time it's used you'll get an OAuth prompt — authorize in the browser once and the connection is stored. You can check the status or re-authenticate any time with `/mcp`.

If you have the `gh` CLI installed and authenticated, the skills will use that instead — the MCP server is the fallback, and it's what makes the plugin work in sandboxed environments (like the desktop app) where terminal commands can't reach `api.github.com`.

## Usage

The skills trigger on natural language, so you don't need to remember command names. In a repo with a GitHub remote, try:

```
turn this decision into an issue
work on issue 14
create a PR for this branch
```

Or invoke them directly: `/github-workflow:create-issue`, `/github-workflow:work-on-issue`, `/github-workflow:create-pr`.

Each skill works out the `owner/repo` from your git remote, so there's nothing to configure — but that also means they only work in a repo that's already on GitHub.

## License

[MIT](LICENSE).
