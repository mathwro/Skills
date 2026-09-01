---
name: choosing-branch-structure
description: Use before starting any persistent repository change when the user has not explicitly chosen a branch, pull-request, worktree, or stacked-branch structure.
license: MIT
---

# Choose branch structure

Choose the delivery topology before the first persistent repository change. Prefer the smallest reviewable structure that preserves the user's explicit intent.

## Inspect

Before deciding, read the request, applicable repository instructions, current branch, and worktree state. If the repository already has an active stack, inspect it before deciding whether the change belongs to one of its layers.

## Decide

Apply the first matching rule:

| Situation | Decision |
| --- | --- |
| No persistent repository change | No branch. |
| User explicitly authorizes direct work on `main` or another named branch, and repository rules permit it | Use that branch. |
| An existing branch or stack layer owns the change | Stay on that branch or layer. |
| Separate concerns can be implemented and reviewed independently | Separate branches or stacks. Use separate worktrees only for concurrent edits. |
| A later concern depends on an earlier concern | One linear `gh-stack`, foundation first. |
| Any other persistent change | One ordinary feature branch and one pull request. |

A request being small never authorizes a direct change to `main`. When the request is ambiguous but does not require a user choice, use an ordinary feature branch. Ask only when the user’s delivery or review intent changes the correct topology.

## Report

State the decision before editing:

```text
Branch decision: <no branch | named branch | existing branch | ordinary branch | linear stack | separate branches>
Reason: <one sentence>
Branches: <names and order, or none>
Worktrees: <current checkout | one per concurrent branch | none>
Next: <one safe action>
```

For a linear stack, list branches trunk-first. Each layer must be independently reviewable and mergeable; place required tests and documentation in the layer they protect.

## Execute the boundary

- For an ordinary branch, create a focused branch using the repository's naming convention. If none exists, use `feature/<topic>`.
- For a linear stack, hand off stack creation, navigation, rebasing, submission, and merge behavior to `gh-stack`.
- For independent work, do not place unrelated changes in the same stack merely because they happen at the same time.
- Do not create a commit, push, pull request, merge, or release merely from this decision. Those actions require their own user instruction and workflow.
- Preserve a dirty worktree. Do not move, discard, stash, or overwrite user changes to force a branch decision.

## Examples

```text
Request: "Fix a typo in the README."
Branch decision: ordinary branch
Reason: The documentation change is persistent and reviewable, but has no dependency boundary.
Branches: feature/readme-typo
Worktrees: current checkout
Next: create the branch, then edit the README.
```

```text
Request: "Add a schema, API, and UI with reviewable PRs."
Branch decision: linear stack
Reason: The API depends on the schema and the UI depends on the API.
Branches: main <- feature/schema <- feature/api <- feature/ui
Worktrees: current checkout
Next: initialize the stack at feature/schema before editing.
```
