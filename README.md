# git-recompose

A Claude Code skill plugin for safely recomposing messy git commits before opening a PR.

## What It Does

When your feature branch has WIP commits, mixed concerns, or unclear messages, this skill helps Claude reorganize the history into clean, logical commits — **without ever touching your original branch** until you explicitly approve.

## Installation

### Universal (works with any coding agent)

```bash
npx skills add nur-zaman/git-recompose-skill
```

Compatible with: Amp, Cline, Codex, Cursor, Gemini CLI, GitHub Copilot, Kimi Code CLI, OpenCode, and more.

### Via Claude Code plugin marketplace

1. Add the marketplace:

```
/plugin marketplace add nur-zaman/git-recompose-skill
```

2. Install the plugin:

```
/plugin install git-recompose@git-recompose-skill
```

### Manual

Copy `skills/recomposing-commits/` into your `~/.claude/skills/` directory:

```bash
cp -r skills/recomposing-commits ~/.claude/skills/
```

## How It Works

1. **Safety check** — refuses to run on `main`, `master`, `dev`, or any protected branch
2. **Analyzes** the commits you want to recompose and identifies logical groups
3. **Isolates** all work in a separate git worktree — your branch is untouched
4. **Stages and commits** each logical group, handling overlapping files with `git add -p`
5. **Review gate** — shows you the new history and waits for your approval
6. **Applies** the recomposed history back only after you say yes, then cleans up

## Triggers

Claude will use this skill when you say things like:

- "clean up my commit history"
- "reorganize commits before PR"
- "recompose my branch"
- "make commits logical"
- "my commits are messy"


## Safety Guarantees

- **Original branch is never modified** until you explicitly approve
- **Refuses protected branches** (`main`, `master`, `dev`, `develop`, `staging`, `production`, `release/*`, `hotfix/*`)
- **Review gate is non-negotiable** — even if you say "skip it", Claude will still show you the new history before applying
- **Verification step** — Claude performs a byte-for-byte diff comparison (not just stats) before presenting the review gate

## What Claude Won't Do

- Use `git rebase -i` (requires interactive TTY — hangs in this context)
- Use `git add -i` (same problem)
- Work directly on the original branch
- Skip the review gate
- Use relative worktree paths (breaks with branch names containing slashes)

## Acknowledgements

Inspired by [GitLens's "Recompose Commits" feature](https://www.gitkraken.com/gitlens).

## License

MIT
