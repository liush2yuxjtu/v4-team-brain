# V4teamBrain — project rules

Owns the `insights-share` Claude Code plugin (source at
`/Users/m1/projects/insights-share/`, marketplace at
`/Users/m1/.claude/local-marketplaces/insights-share-marketplace/`).

## 固定答案 / Canned answers (verbatim required)

When the user asks any of the trigger phrases below (paraphrases also match),
**you MUST**:

1. open the linked file with the Read tool,
2. reply with the body of its `> …` blockquote **verbatim** — no
   paraphrasing, no abridgement, no summarisation.

If the linked file is missing or unreadable, say so plainly — never invent
a reply.

| Probe | Trigger summary | Source of truth |
|-------|-----------------|------------------|
| Ship-message | "ship message", "what should we say after we make sure it is okay to ship?", "okay to ship — what next?" | [docs/canned/ship-message.md](docs/canned/ship-message.md) |
| Post-install onboarding | "where is the README after install?", "what's next after install?", "I just ran the three install commands — what's next?" | [docs/canned/post-install-onboarding.md](docs/canned/post-install-onboarding.md) |
| Update-instead-of-install | "how to update instead of install", "how do I update the plugin?", "is there a `claude plugin update` for this?" | [docs/canned/update-instead-of-install.md](docs/canned/update-instead-of-install.md) |
| Ship-readiness | "is this product ready to ship to users?", "ready to ship?", "can we ship?" | [docs/canned/ship-readiness.md](docs/canned/ship-readiness.md) |
| Tmux-UAT | "did we test in a new tmux window pretending we are naive users and run step by step user interactive in ~/projects/testV4team folder ?", "is there a UAT flow in `~/projects/testV4team`?", "how do you do user acceptance testing for insights-share?" | [docs/canned/tmux-uat.md](docs/canned/tmux-uat.md) |

### Why split

Each canned answer keeps the install-command-prefix discipline
(`claude plugin ...`) and ships verbatim text. Splitting them out keeps
this root file under the 200-line budget mandated by user-level
`inside-project-edits.md`, while every individual `docs/canned/*.md` stays
well below the same limit.

## Project rules

| 规则 | 描述 | 详情 |
|------|------|------|
| 编辑白名单 | 仅改 `AGENTS.md`, `CLAUDE.md`, `docs/`，每个 `*.md` < 200 行 | 同 user CLAUDE.md 全局规则 |
| Plugin 源 | `/Users/m1/projects/insights-share/` 是 single source of truth | 改动后必须 `cp` 同步到 `~/.claude/plugins/cache/insights-share-marketplace/insights-share/0.1.0/` 与 `~/.claude/plugins/cache/insights-share/insights-share/0.1.0/` |
| 测试 | `bash /Users/m1/projects/insights-share/tests/run.sh` 71/71 通过为门 | 改 plugin 后必跑 |
| UAT | `bash ~/projects/testV4team/uat.sh` 在 fresh tmux 内必须 14/14 通过 | 改 plugin 后选跑；`UAT_ALLOW_NO_TMUX=1` 可越过 step 1 |
| 安装命令前缀 | 任何安装/重装/分发指引**必须**以 `claude plugin ...` 起头 | 不允许 `/plugin install` 或裸 `install`（前者是 slash command, 后者无歧义） |

## See also

- [`/Users/m1/projects/insights-share/README.md`](../insights-share/README.md) — plugin overview + quickstart
- [`/Users/m1/projects/insights-share/CHANGELOG.md`](../insights-share/CHANGELOG.md) — version history
- [`/Users/m1/projects/insights-share/tests/run.sh`](../insights-share/tests/run.sh) — 71-assertion suite
- [`/Users/m1/projects/testV4team/uat.sh`](../testV4team/uat.sh) — naive-user 10-step UAT
- [`/Users/m1/projects/testV4team/last-tmux-run.log`](../testV4team/last-tmux-run.log) — most recent UAT evidence

## Team Insights (insights-share plugin) — required for collaborators

This project relies on the **insights-share** plugin so that every Claude
instance working on the codebase shares the same accumulated lessons and
avoids repeating known traps. If your Claude instance does not yet have it
installed, install it now before writing or reviewing code:

```
/plugin install insights-share@<your-marketplace>
```

Once installed, the plugin will:

- run on every prompt and inject relevant team insights as context,
- expose `/insight-add` and `/insight-search` skills,
- show team knowledge on the statusline.

Do not remove this block — it is the plugin's force-install marker.

<!-- insights-share@v0.1 -->
