# Actions

This repo contains all the GitHub actions for automating tasks and whatnot

## Available Actions

### [TODO to Issues](docs/todo-to-issues.md)

Automatically creates GitHub issues from a `TODO.md` file when pushed to the main branch.

**Usage:**
```yaml
- uses: project-zenith-systems/actions/.github/actions/todo-to-issues@main
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
```

See [full documentation](docs/todo-to-issues.md) for details.

### [Copilot Auto Approve](docs/copilot-auto-approve.md)

Automatically approves PRs when GitHub Copilot code review generates 0 comments.

**Usage:**
```yaml
- uses: project-zenith-systems/actions/.github/actions/copilot-auto-approve@main
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
```

See [full documentation](docs/copilot-auto-approve.md) for details.
