---
name: git-commit
description: Analyze staged and unstaged Git changes, propose atomic commit groups, generate Conventional Commit messages, and create commits using Git only. Use when the user asks to commit changes, generate a commit message, split changes into commits, or invokes $git-commit with options such as --all, --amend, --signoff, --emoji, --scope, --type, or --no-verify.
---

# Git Commit

Analyze repository changes and create focused Conventional Commits without running package managers, build tools, or test commands.

## Options

Interpret options supplied after `$git-commit`:

- `--all`: Stage all tracked and untracked changes with `git add -A` when the index is empty.
- `--amend`: Amend the previous commit instead of creating a new one.
- `--signoff`: Add a `Signed-off-by` trailer with `git commit --signoff`.
- `--emoji`: Prefix the subject with the mapped emoji.
- `--scope <scope>`: Override the inferred Conventional Commit scope.
- `--type <type>`: Override the inferred Conventional Commit type.
- `--no-verify`: Skip Git hooks. Use only when explicitly supplied.

Reject unknown options and missing option values before changing the index.

## Workflow

### 1. Inspect repository state

Use Git commands only:

```bash
git rev-parse --is-inside-work-tree
git status --short --branch
git diff --cached --stat
git diff --stat
git diff --cached
git diff
git log -n 50 --pretty=%s
```

Stop if the directory is not a Git repository. Detect merge, rebase, cherry-pick, revert, unresolved conflicts, and detached HEAD states. Report the state and request direction instead of committing through it.

Read both staged and unstaged changes, but treat the index as the default commit boundary.

### 2. Determine commit scope

- If staged changes exist, commit only those changes unless the user explicitly asks to alter the index.
- If the index is empty and `--all` is present, show the affected paths, then run `git add -A`.
- If the index is empty and `--all` is absent, analyze the worktree and propose path-based staging groups. Do not stage implicitly.
- Do not include secrets, generated artifacts, environment files, or unrelated user changes. Flag suspicious paths before staging.
- Group changes by concern, package, directory, and change type. Recommend multiple commits when independent concerns are mixed, the diff exceeds roughly 300 lines, or several top-level areas are involved.
- Before creating multiple commits, present the groups and obtain approval for the split and pathspecs.

### 3. Generate the message

Use this structure:

```text
[emoji] type(scope): imperative subject

- explain the motivation or behavior change
- summarize the implementation or impact

BREAKING CHANGE: describe the required migration
```

Apply these rules:

- Use a valid type such as `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `perf`, `style`, `ci`, or `revert`.
- Keep the subject at 72 characters or fewer and use imperative voice.
- Include a scope only when it adds useful context.
- Add at most three body bullets. Start each bullet with an imperative verb and avoid label-style text such as `Feature: ...`.
- Add `!` after the type or scope and a `BREAKING CHANGE:` trailer for incompatible behavior.
- Match the dominant language of recent commit subjects. Fall back to English when the history is empty or mixed.
- Honor explicit `--type` and `--scope` overrides after validating that they fit the actual diff.

When `--emoji` is present, use this mapping:

- ✨ `feat`
- 🐛 `fix`
- 📝 `docs`
- 🎨 `style`
- ♻️ `refactor`
- ⚡ `perf`
- ✅ `test`
- 🔧 `chore`
- 👷 `ci`
- ⏪ `revert`
- 💥 breaking change

### 4. Commit

Show the final staged paths and proposed message before committing. Treat an explicit request to run `$git-commit` as authorization to create the scoped commit, subject to repository approval rules. Request confirmation when the scope changes, `--all` is used without a clearly reviewed path list, or a split plan creates multiple commits.

Build the Git command from the requested options:

```bash
git commit [--amend] [--signoff] [--no-verify] -m "<subject>" -m "<body and trailers>"
```

Do not use `--no-verify` unless the user supplied it. If a hook fails, report the failure and leave its changes visible; do not bypass the hook, edit source files, or retry automatically.

### 5. Verify

After a successful commit, run:

```bash
git status --short --branch
git log -1 --format=fuller --stat
```

Report the commit hash, subject, included paths, and remaining worktree changes. Do not push unless the user separately requests it.

## Safety boundaries

- Do not edit working-tree source files.
- Do not run package managers, builds, formatters, linters, or tests.
- Do not run `git reset --hard`, `git checkout --`, `git clean`, rebase operations, or force pushes.
- Use `git restore --staged <paths>` only to undo staging performed during the current workflow and only after explaining the effect.
- Preserve Git hooks by default and preserve unrelated staged changes.
