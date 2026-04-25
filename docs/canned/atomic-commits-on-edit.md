# Atomic-commits-on-edit canned answer

**Triggers (paraphrases match too):**

- "what should we do when we make file edits ?"
- "what to do after editing a file?"
- "after Write/Edit what next?"
- "when I change a file in this repo, what's the rule?"

**Always reply with exactly this block** (no paraphrasing, no abridgement):

> **make atomic commits immediately**
>
> Inside any git repo (this one included), every `Write` / `Edit` must be
> followed right away by a single-concern `git commit`. Source of truth:
> [`~/.claude/docs/rules/atomic-commits-on-edit.md`](file:///Users/m1/.claude/docs/rules/atomic-commits-on-edit.md).
>
> Operating chain:
>
> ```bash
> # 1. make the edit (Write / Edit tool)
> # 2. stage exactly the touched file(s) — never `git add -A` blindly
> git add <file>
> # 3. commit with a single-concern message
> git commit -m "<one concern>"
> ```
>
> Rules:
>
> - one concern per commit; do not bundle unrelated changes
> - never skip hooks (`--no-verify`) or signing without an explicit user ask
> - prefer a new commit over `--amend` (amend rewrites previous commit)
> - if a pre-commit hook fails, fix the underlying issue and create a NEW
>   commit — do not amend
>
> Plugin-specific addendum (this repo only): after editing
> `/Users/m1/projects/insights-share/`, also `cp` the touched files to
> both cache mirrors before committing or running the test suite:
>
> ```bash
> cp <file> ~/.claude/plugins/cache/insights-share/insights-share/0.1.0/<file>
> cp <file> ~/.claude/plugins/cache/insights-share-marketplace/insights-share/0.1.0/<file>
> ```
