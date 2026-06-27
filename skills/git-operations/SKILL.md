---
name: git-operations
version: 1.0.0
category: devops
description: Git workflow automation — clone, branch, commit, PR, merge via GitHub CLI (gh).
tags: [git, github, gh, version-control, pr, branch]
---

# Git Operations

Standardized Git workflows using the `gh` CLI for GitHub operations. Covers: clone, branch management, commit, PR creation, merge, and repo inspection.

## When to Activate

- User asks to clone, branch, commit, PR, or merge code
- GitHub repo inspection or file listing
- Automated git workflows in CI/CD contexts

## Workflow

### Clone a Repo

```bash
gh repo clone <owner>/<repo> [-- --depth 1]
```

### Branch Management

```bash
gh repo create <name> [--public|--private] [--clone]
git checkout -b feature/my-branch
```

### Commit & Push

```bash
git add .
gh repo commit -m "feat: description" --amend  # interactive
git push -u origin HEAD
```

### Pull Request

```bash
gh pr create \
  --title "feat: my feature" \
  --body "## What\n- Description" \
  --label "enhancement" \
  --reviewer <user1>,<user2>
```

### PR Merge

```bash
gh pr merge <pr-number> --squash --delete-branch
```

### Inspect Repo

```bash
gh repo list <owner> --limit 10
gh repo view <owner>/<repo> --json description,pushedAt
gh issue list --repo <owner>/<repo> --state open --limit 5
```

## Token Auth

Ensure `~/.gh_token` exists or run `gh auth login`. Token needs `repo` scope for private repos.

## Anti-patterns

- ❌ `git push --force` on main/master
- ❌ Committing secrets or large binary files
- ❌ Not running `gh auth status` before private repo operations
