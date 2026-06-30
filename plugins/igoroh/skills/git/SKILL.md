---
name: git
description: Learn user's git workflow preferences. Use always when you need to make a change to git related entity/resourse (PR/Branch/Commit etc.) and before coding tasks.
---

## Introduction

Your main goal in working with git must be to make the review process easier for reviewers.
All the rules presented in this skill serve this goal.

Commit care exists for the review, not for main's history — PRs are squash-merged, so the split commits live only as long as the PR.

## Commits

Split a task into self-describing commits to enable an easy review process through commit history.

Splitting rules:
- A mechanical refactor never shares a commit with a behavior change.
- Tests land in the same commit as the code they test.

Message conventions:
- Imperative mood, sentence case, subject ≤72 chars (e.g. `Add Vite + React admin panel SPA`).
- No conventional-commit prefixes (`feat:`, `fix:`).
- No ticket number in the subject — it lives in the branch name.
- Body only when the *why* isn't obvious from the diff.

History rewriting is two-phase:
- **Before the first review**: rebase/reorder freely to keep commits clean.
- **After review starts**: never rewrite published history — append `fixup!` commits instead. No autosquash is needed before user feedback.

## Branches

Branch name starts with the ticket number (e.g. `ag-51-...`), kebab-case. Ask the user for the ticket number if not provided. If there is no ticket, use a reasonable kebab-case name.

### Stacked branches (git-spice)

Propose a stack — never start one silently — when the task decomposes into independently-deliverable **vertical slices**: each slice cuts through all layers (data model → API → UI) to deliver one complete capability. Classic CRUD split: read slice first (model + list/get + list view), then create, then edit/delete — each slice is demoable and reviewable end-to-end. Don't slice by layer (schema PR, then API PR, then FE PR); a layer-only branch is justified only when that layer itself carries the risk (e.g. a hairy migration). A diff above ~500 non-generated lines is a secondary signal, not the trigger.

Use the `gs` CLI for all stack operations; see [references/git-spice.md](references/git-spice.md). Don't mix raw `git rebase` into a tracked stack — it desyncs spice metadata.

## PRs

When you finish a task, open a **draft** PR. Specify the user as an assignee.

Title: imperative, same style as commit subjects, no ticket prefix. The PR is squash-merged, so the title becomes the commit on main — make it good.

Description structure:

1. `Ticket: <notion-url>` — when a ticket exists; ask for the URL if it wasn't given.
2. **Goal**
3. **Changes** — a concise description of what was changed.
4. **Notes** — optional; anything else worth mentioning (tradeoffs, follow-ups, gotchas). Omit when there's nothing to add.
5. **How to review** — suggested commit-by-commit reading order and where the risk concentrates. Only for PRs with more than 3 commits.
6. **Screenshots** — only when the PR contains frontend work. See [Screenshots](#screenshots).

## Screenshots

GitHub has no API for attaching images, so upload screenshots to Cloudflare R2 and embed the public links in the PR description.

Upload each image with the `wrangler` CLI (run as `bunx wrangler`; an authenticated session is assumed):

```
bunx wrangler r2 object put "github-screenshots/<key>" --file=<path> --content-type=image/png --remote
```

- Bucket: `github-screenshots`. Public base URL: `https://pub-07cc2e1863834d319b66aa5802c66ea3.r2.dev/<key>`.
- `--remote` is required — without it wrangler writes to the local simulator, not R2.
- The key **must** include a random component so the public URL isn't guessable (the bucket is world-readable with no listing, so an unguessable key is the only thing protecting the image). Use e.g. `<branch>/<random-hex>.png` — generate the suffix with `openssl rand -hex 4`.

Always present screenshots in a markdown table, never as bare images:

```
| Before | After |
| --- | --- |
| ![before](https://pub-07cc2e1863834d319b66aa5802c66ea3.r2.dev/ag-51-a1b2c3d4.png) | ![after](https://pub-07cc2e1863834d319b66aa5802c66ea3.r2.dev/ag-51-e5f6a7b8.png) |
```

Pick table columns that fit the change (Before/After, one column per screen, etc.). After uploading, fetch each URL once to confirm it returns `200` before relying on it in the description.
