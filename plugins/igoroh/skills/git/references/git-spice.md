# git-spice cheat sheet

CLI is `gs`. Aliases combine: `gs bc` = `gs branch create`, `gs cc` = `gs commit create`.

## Creating a stack

```sh
gs repo init                 # once per repo (no-op if done)
gs branch create ag-51-api   # new branch stacked on the current one
gs commit create             # commit; like git commit, but spice-aware
gs branch create ag-51-ui    # next layer, stacked on ag-51-api
```

`gs branch create` stacks on whatever branch you're on — start from `main` (or `gs trunk`) for the first layer.

## Navigation

```sh
gs up / gs down          # move one branch up/down the stack
gs top / gs bottom       # jump to ends of the stack
gs trunk                 # jump to main
gs branch checkout       # interactive picker
gs log short             # show the stack (gs ls)
```

## Changing earlier layers

```sh
gs branch checkout <lower-branch>   # go to the layer to fix
gs commit create                    # or: gs commit amend / gs commit fixup
gs upstack restack                  # rebase everything above onto the change
```

`gs commit amend` and `gs commit create` auto-restack the upstack in most cases; run `gs stack restack` if anything reports out-of-sync. If a rebase stops on conflicts: resolve, then `gs rebase continue` (not raw `git rebase --continue`).

## Submitting PRs

```sh
gs stack submit          # PRs for every branch in the stack (bases chained)
gs branch submit         # just the current branch
gs downstack submit      # current branch and everything below
```

Re-running submit updates existing PRs (force-push + retarget) instead of creating new ones.

## After a PR merges (squash-merge friendly)

```sh
gs repo sync             # pull trunk, detect merged PRs, delete their branches
gs stack restack         # restack what remains onto the new trunk
```

`gs repo sync --restack` does both in one go.

## Cautions

- Don't use raw `git rebase`/`git commit --amend` on tracked branches — spice loses track of branch relationships. Use the `gs` equivalents.
- `gs branch fold` merges a branch into its base; `gs branch split` breaks one branch into layers — useful when a single branch grew into a natural stack after the fact.
