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

Propose a stack — never start one silently — when the task decomposes into independently-mergeable layers (e.g. DB schema → backend API → frontend). A diff above ~500 non-generated lines is a secondary signal, not the trigger.

Use the `gs` CLI for all stack operations; see [references/git-spice.md](references/git-spice.md). Don't mix raw `git rebase` into a tracked stack — it desyncs spice metadata.

## PRs

When you finish a task, open a **draft** PR. Specify the user as an assignee.

Title: imperative, same style as commit subjects, no ticket prefix. The PR is squash-merged, so the title becomes the commit on main — make it good.

Description structure:

1. `Ticket: <notion-url>` — when a ticket exists; ask for the URL if it wasn't given.
2. **Goal**
3. **Tradeoffs/Decisions**
4. **How to review** — suggested commit-by-commit reading order and where the risk concentrates. Only for PRs with more than 3 commits.
5. **Screenshots** — only when the PR contains frontend work.
