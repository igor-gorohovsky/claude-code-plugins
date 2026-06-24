---
name: work-process
description: Learn user's work process to complete tasks. Use when the user gave you a task and you are going to explore/discuss/implement.
---

# Skill Work Process

This skill provides guidance to make work with me more productive.

# Step 0 — Triage

Size the task before anything else:

- **Trivial** (typo, config tweak, obvious one-liner): just do it and commit. Skip the rest of this process.
- **Small** (clear approach, single slice, roughly < half a day): post a short inline plan — a few sentences, not `grill-me` — and proceed immediately without waiting for sign-off; the user will interrupt if it's wrong. Then continue from Step 2.
- **Large** (ambiguous, multi-slice, or stack-worthy per the `git` skill): full pipeline below.

# Step 1 — Explore, then Plan & Discuss

Explore the affected code _before_ the discussion — come with findings and a draft approach, not open questions the codebase can answer.

Then discuss the plan using the `grill-me` skill.
Do not use AskUserQuestion with the `grill-me` skill.

# Step 2 — Implementation

Implement the discussed plan.
Use `tdd` for new behavior. Skipping is fine for pure refactors covered by existing tests, config changes, and UI polish — but say explicitly that you're skipping and why.
Follow the `git` skill for branches and commits.

# Step 3 — Verify & Open PR

Definition of done before the PR:

- Tests green, lint clean.
- `task codegen` rerun if the backend API surface changed.
- `/verify` (run the app and observe the change) — only for user-visible changes; skip for refactors and internal plumbing.

Open a draft PR per the `git` skill.

# Step 4 — Code Review

Run `/code-review` at **max** effort with `--comment` and `--fix` so findings land on the PR. Apply the fixes as `fixup!` commits and leave them withut autosquash. When you will fix findings reply to the leaved comment with link to commit and resolve the thread.

# Step 5 — Reflect

If this task surfaced new workflow friction or a rule worth keeping, propose a one-line addition to the `git` or `work-process` skill.
