---
name: commit-and-document
description: Finalize completed repository work by auditing relevant agent instructions and documentation, updating them when the changes require it, validating the result, and committing the intended changes. Use this skill only when the user explicitly asks to commit, finalize, check in, or commit-and-document completed changes; do not trigger merely because a repository has uncommitted work.
---

# Commit and document

Finalize the user's completed changes as one safe, reviewable commit. This skill is user-invoked only: do not start it from an unprompted request to edit code, or merely from seeing a dirty worktree.

## Procedure

1. Confirm the working directory is inside a Git repository with `git rev-parse --show-toplevel`. If it is not, report that the requested commit cannot be made.
2. Inspect the complete worktree state before changing anything:
   - Run `git status --short`.
   - Review both staged and unstaged changes with `git diff HEAD` and the names of untracked files.
   - Inspect the relevant surrounding files, recent commit subjects, and repository guidance so the commit and documentation match local conventions.
3. Establish scope. Use the user's request and the diff to identify the changes that belong in this commit. Preserve unrelated user work. Never use `git reset`, `git clean`, checkout-based discards, rebases, or amend an existing commit to hide unrelated changes.
   - If intended and unrelated changes are mixed in the same file and cannot be separated safely, stop and ask the user which content belongs in the commit.
   - Do not commit secrets, credentials, private keys, local environment files, or accidental generated output. Report any concerning findings and prompt the user if they should be removed or included.
4. Audit repository guidance and documentation before staging:
   - Find `AGENTS.md`, `agents.md`, `CLAUDE.md`, and `claude.md` files. For each changed path, apply the nearest applicable instructions and update the applicable agent file when the change introduces or changes durable repository guidance, commands, architecture, conventions, or workflow knowledge. Do not edit an agent file just to mention an ordinary implementation detail.
   - Find the repository's README files and other relevant documentation such as `docs/`, `CONTRIBUTING`, changelogs, and architecture or API documents. Update the README when the change affects user-visible setup, usage, behavior, or public interfaces. Update other documents when their documented behavior is now inaccurate or the new workflow belongs there. Preserve the repository's existing style.
   - Do not mass-edit every Markdown file and do not invent documentation for an internal change with no durable user or maintainer impact. If no documentation needs updating, record the reason in the final report.
5. Run the narrowest relevant validation after documentation edits: the repository's documented tests, build, lint, type check, or smoke command for the changed behavior. If no suitable command exists, inspect the final diff carefully. Do not bypass a required check. If a check fails, fix an in-scope cause when possible; otherwise report the failure and do not claim successful finalization.
6. Stage only the intended paths explicitly. Review the staged patch with `git diff --cached`, checking that documentation changes are included, unrelated changes are absent, and no sensitive data is staged.
7. Create a concise commit using the repository's commit-message convention. The message must describe the actual completed change; do not use an empty commit. If the user supplied a message, preserve its intent unless it violates repository policy.
8. Verify the result with `git show --stat --oneline HEAD` and `git status --short`. Confirm the new commit contains the intended source and documentation updates. Report the commit identifier, subject, validation performed, documentation files changed (or why none were needed), and any remaining unrelated worktree changes.

## Safety boundaries

- Do not commit without the user's explicit invocation of this skill.
- Do not fabricate documentation updates, validation results, or a clean worktree.
- Do not rewrite history, discard work, bypass hooks, or use force options as part of normal finalization.
- A pre-existing dirty worktree is not permission to commit every file. Scope and stage deliberately.
- If hooks fail, treat the commit as incomplete until the issue is resolved or the user gives a separate explicit instruction to accept the failure.
