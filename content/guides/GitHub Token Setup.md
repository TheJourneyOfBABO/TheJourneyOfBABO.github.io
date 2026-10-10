---
title: GitHub Token Setup
description: "Create a personal access token and use it for Git over HTTPS"
tags:
  - guide/git
date: 2025-08-09
---
GitHub no longer accepts account passwords for Git operations over HTTPS. Use a personal access token instead.

## Generate Token
1. Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Click "Generate new token"
3. Select `repo` scope
4. Generate and copy the token (you won't see it again)

## Use Token
When Git asks for password, use the token instead:
```bash
Username: your-github-username
Password: your-token-here
```

## Save Credentials (Optional)
To avoid entering credentials every time:
```bash
# Store permanently
git config --global credential.helper store

# OR cache temporarily (15 minutes)
git config --global credential.helper cache
```

> [!warning]
> `store` saves the token in plain text in `~/.git-credentials`. On shared machines, use `cache` instead.

## Security Tips
- Never share your token
- Don't commit it to repositories
- Delete unused tokens from GitHub settings

## See also
- [[Terminal Setup Advanced|Terminal Setup: Advanced]] — Lazygit for a visual Git workflow
