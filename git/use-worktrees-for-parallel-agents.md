# Use git worktrees for parallel agents/sessions

When two processes (two terminals, two AI coding agents, two CI steps) operate on the *same* working directory, they share one `HEAD`. The moment one of them runs `git checkout`/`git switch`, the other's next commit lands on the wrong branch — silently.

This bit me repeatedly when running parallel automation against one repo: work meant for a feature branch ended up committed onto `main`, which then diverged and stale-based every open PR.

## Fix

Give each parallel worker its own working tree off the same `.git`:

```bash
git worktree add ../repo-feature-x -b feature/x   # new branch in a sibling dir
# ...work there...
git worktree remove ../repo-feature-x             # clean up when done
git worktree prune                                # drop stale registrations
```

Each worktree has an independent `HEAD` and index but shares the object store, so it's cheap (no second clone) and you can't cross-contaminate branches.

## Takeaway

Never point two concurrent writers at one working directory. Worktrees are the right primitive for "same repo, different branch, at the same time" — and they clean up with `git worktree remove`.
