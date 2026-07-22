---
description: Automatically stage, commit, and push all new stories, craft notes, and indexes to GitHub repository.
argument-hint: <optional custom commit message | blank for automatic commit message>
allowed-tools: Bash, Read, Write, run_command
---

Automatically sync the **stories** repository with GitHub (`https://github.com/chandanpanda615-lab/stories`).

## Execution Steps

1. Check `git status` in the repository root.
2. If there are no changes or untracked files, report: *"Repository is already up to date with GitHub."* and stop.
3. If `$ARGUMENTS` is provided, use `$ARGUMENTS` as the commit message.
4. If `$ARGUMENTS` is empty, inspect the changed files and generate a logical commit message based on which writer folders changed (e.g. `docs(anulata): update story craft notes` or `feat(stories): add new writer lessons`).
5. Run `git add .`.
6. Run `git commit -m "<commit message>"`.
7. Run `git push origin main`.
8. Report back with the commit hash, changed files, and confirmation of successful push to `https://github.com/chandanpanda615-lab/stories`.
