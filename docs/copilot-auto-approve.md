# Copilot Auto Approve Action

Automatically approves pull requests when GitHub Copilot code review generates 0 comments.

## Features

- Triggers on pull request review events
- Detects when Copilot's code review generates 0 comments
- Automatically approves the PR with an informative message
- Ignores reviews from other users

## How It Works

When GitHub Copilot performs a code review on a pull request and finds no issues, it posts a comment like:

```
GitHub Copilot code review generated 0 comments.
```

This action listens for pull request review events and:
1. Checks if the reviewer is `copilot-pull-request-reviewer[bot]`
2. Checks if the review body contains "generated 0 comments"
3. If both conditions are met, automatically approves the PR

## Usage

To use this action in your repositories:

1. Create a `.github/workflows/copilot-auto-approve.yml` file:

```yaml
name: Copilot Auto Approve

on:
  pull_request_review:
    types: [submitted]

jobs:
  auto-approve:
    runs-on: ubuntu-latest
    permissions:
      pull-requests: write
    
    steps:
      - name: Auto approve if Copilot finds no issues
        uses: project-zenith-systems/actions/.github/actions/copilot-auto-approve@main
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `github-token` | GitHub token (for example, `secrets.GITHUB_TOKEN`, a PAT, or a GitHub App token) used to approve PRs | Yes | `${{ github.token }}` |

## Notes

- The action only triggers on `pull_request_review` events with the `submitted` type
- A GitHub token with `pull-requests: write` permission is required, and your repository/organization must allow GitHub Actions to create and approve pull requests.
- If the workflow fails with `403 Resource not accessible by integration`, ensure that setting is enabled or provide a PAT/GitHub App token with appropriate pull request write permissions (for example, the "Pull requests: write" permission) via the `github-token` input.
- The approval message includes context about why the PR was approved
- Reviews from users other than Copilot are ignored
- Reviews with comments (not 0) are also ignored
