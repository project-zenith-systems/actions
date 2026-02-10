# TODO to Issues Action

Automatically creates GitHub issues from a `TODO.md` file when pushed to the main branch.

## Features

- Automatically creates GitHub issues from TODO items
- Supports optional descriptions for each TODO
- Deletes TODO.md after processing to avoid duplicates
- Prevents concurrent runs to avoid duplicate issues
- Simple and intuitive TODO.md format

## TODO.md Format

The format supports both headlines and bullet points:

```markdown
## Issue Title

Optional description for the issue.
Can span multiple lines.

## Another Issue Title

Another optional description.

## Issue With Sub-tasks

- [ ] First sub-task
- [ ] Second sub-task
- Bullet without checkbox

## Issue Without Description

- [ ] Standalone bullet (becomes its own issue)
- [x] Completed item (skipped)
```

### Headlines (`## `)

Each `## ` heading becomes an issue title. Any text between it and the next heading is the issue body. Bullet points under a headline are included in that issue's body rather than creating separate issues.

### Bullet points (`- [ ] ` or `- `)

Standalone bullet points (not under a `## ` heading) each create their own issue. Completed items (`- [x] `) are always skipped.

## Usage

To use this action in your repositories:

1. Create a `.github/workflows/process-todos.yml` file:

```yaml
name: Process TODOs

on:
  push:
    branches:
      - main
    paths:
      - 'TODO.md'

# Prevent concurrent runs to avoid duplicate issues
concurrency:
  group: process-todos
  cancel-in-progress: false

jobs:
  create-issues:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      issues: write
    
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Process TODO file
        uses: project-zenith-systems/actions/.github/actions/todo-to-issues@main
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

2. Create a `TODO.md` file in your repository root with your TODO items
3. Push to main branch
4. The action will create issues and delete the TODO.md file

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `github-token` | GitHub token for creating issues | Yes | `${{ github.token }}` |
| `todo-file` | Path to the TODO file | No | `TODO.md` |

## Example TODO.md

See [TODO.example.md](../../.github/actions/todo-to-issues/TODO.example.md) for a complete example.

```markdown
## Add user authentication

Implement OAuth2 authentication flow with support for:
- Google login
- GitHub login
- Email/password fallback

## Improve error handling

Add better error messages and logging throughout the application.

## Update documentation
```

This will create 3 issues with the titles and descriptions as specified.

## How It Works

1. When you push a TODO.md file to the main branch
2. The workflow triggers and checks for the TODO.md file
3. It parses the file looking for `## ` headings and bullet points
4. Bullet points under a heading are included in that heading's issue body
5. Standalone bullet points (not under a heading) create their own issues
6. Completed items (`- [x]`) are skipped
7. All issues are labeled with `TODO.md`
8. Issues are created via the GitHub API
9. The TODO.md file is deleted with a commit
10. Concurrency control prevents duplicate runs

## Notes

- The action uses `## Title` for issues with descriptions and `- [ ] Item` for quick items
- Bullet points under a `## ` heading become part of that heading's issue body
- Descriptions are optional and can include blank lines, special characters, etc.
- The TODO.md file is automatically deleted after processing
- Concurrency control prevents race conditions and duplicate issues
- The action is reusable and can be used in any repository
