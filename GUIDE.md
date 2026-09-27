# Claude Code — 15-Minute Power-User Guide

The 20% of Claude Code that gets you 80% of the value. Read once; keep as reference.

## 1. The memory file (`CLAUDE.md`) is your #1 lever
Claude Code loads `CLAUDE.md` from the project root (and `~/.claude/CLAUDE.md` globally) into every
session. A good one is the difference between generic and expert help. Put in it:
- **How to run things** — build, test, lint, dev-server commands (exact invocations).
- **Conventions** — formatting, naming, commit style, branch rules.
- **Architecture** — the 5-sentence map of where things live.
- **Gotchas** — the non-obvious rules ("never edit `generated/`", "DB migrations are manual").
Keep it tight. It's context tax on every turn — prune ruthlessly. Use the templates in `templates/`.

## 2. Slash commands = reusable prompts
Any `.md` file in `.claude/commands/` becomes `/name`. Frontmatter controls it:
```md
---
description: One line shown in the menu
argument-hint: <pr-number>
allowed-tools: Bash(git status:*), Bash(git diff:*)
---
Your prompt. Use $ARGUMENTS for everything after the command, or $1 $2 for positionals.
```
Repo-level commands live in `.claude/commands/`; personal ones in `~/.claude/commands/`. See `commands/`.

## 3. Permissions: stop approving the same thing 100 times
In `.claude/settings.json`, pre-approve safe, common operations so you're only prompted for the risky ones:
```json
{
  "permissions": {
    "allow": ["Bash(npm run test:*)", "Bash(git status)", "Read", "Edit"],
    "ask":   ["Bash(git push:*)"],
    "deny":  ["Bash(rm -rf:*)", "Read(./.env)", "Read(./.secrets/**)"]
  }
}
```
`allow` runs without asking, `ask` always confirms, `deny` blocks outright. Deny reads of secrets.

## 4. Hooks: deterministic guardrails
Hooks run *your* shell commands on lifecycle events (before/after a tool, on stop, etc.). Use them for
things you want to happen *every time* regardless of the model: auto-format after edits, block commits
that contain secrets, run a smoke test after code changes. See `hooks/`.

## 5. Plan before big changes
For anything non-trivial, ask Claude to **produce a plan first** and approve it before it edits. It
catches wrong assumptions cheaply. (Plan mode / "make a plan, don't write code yet" both work.)

## 6. Context hygiene
- `/clear` between unrelated tasks — a stale context makes worse decisions.
- `/compact` at natural breakpoints on long tasks to summarize and keep going.
- Point at specific files/paths instead of "the codebase" — narrower context = sharper answers.

## 7. MCP: give Claude your tools
Model Context Protocol servers add tools (databases, issue trackers, browsers, cloud APIs). Add them in
settings; then Claude can query your DB or open PRs directly. Start with one you'll actually use.

## 8. A tight daily loop
1. `/clear` → state the goal in one sentence → point at the files.
2. Ask for a plan on anything bigger than a one-liner.
3. Let hooks format/lint; you review the diff, not the typing.
4. `/commit` when green. `/review` before you push.

That's the whole game: **great memory file, reusable commands, tight permissions, deterministic hooks,
plan-first, clean context.**
