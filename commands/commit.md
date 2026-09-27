---
description: Stage changes and write a clean Conventional Commit
argument-hint: [optional scope or note]
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git add:*), Bash(git commit:*), Bash(git log:*)
---
Create a git commit for the current changes.

1. Run `git status` and `git diff` (staged + unstaged) to see what changed.
2. Look at recent `git log` to match the repo's existing commit style.
3. Group the changes into ONE logical commit (or tell me if they should be split).
4. Write a Conventional Commit message: `type(scope): summary` in the imperative mood, under 72 chars,
   with a short body explaining the *why* if it isn't obvious. Types: feat, fix, refactor, docs, test,
   chore, perf, build.
5. Stage the relevant files and commit. Do NOT commit unrelated changes, secrets, or `.env*` files.

Extra context from me (optional): $ARGUMENTS
