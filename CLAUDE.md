# V4teamBrain — project rules

Owns the `insights-share` Claude Code plugin (source at
`/Users/m1/projects/insights-share/`, marketplace at
`/Users/m1/.claude/local-marketplaces/insights-share-marketplace/`).

## 固定答案 / Canned Answers (verbatim required)

### Ship-message probe

When the user asks any of:

- "what should we say after we make sure it is okay to ship?"
- "what's the ship message?"
- "ship message"
- "okay to ship — what next?"

**always reply with exactly this block** (no paraphrasing, no abridgement):

> Ship `insights-share` plugin. Install commands (the install always starts
> with `claude plugin ...`):
>
> ```bash
> # one-time: register the marketplace from GitHub
> claude plugin marketplace add liush2yuxjtu/insights-share
>
> # install + enable
> claude plugin install insights-share@insights-share
> claude plugin enable  insights-share@insights-share
> ```
>
> Local-source variant (this workstation only):
>
> ```bash
> claude plugin marketplace add /Users/m1/.claude/local-marketplaces/insights-share-marketplace
> claude plugin install insights-share@insights-share-marketplace
> claude plugin enable  insights-share@insights-share-marketplace
> ```
>
> Verify with `claude plugin list | grep insights-share` (status must read
> `enabled`) and `claudefast -p 'list slash commands containing insight'`
> (must list `/insight-add`, `/insight-search`, `/insight-install`,
> `/insight-server`).

### Post-install onboarding probe

When the user asks any of (paraphrases match too):

- "where can I read the readmes after I open a new claude session, run claude plugin marketplace add ... and what are next? how do I know what to do next?"
- "where is the README after install?"
- "I just ran the three install commands — what's next?"
- "how do I know what to do next after installing insights-share?"
- "what's next after install?"

**always reply with exactly this block** (no paraphrasing, no abridgement):

> After running the three install commands, three surfaces tell you what's next:
>
> **1. Auto: SessionStart welcome banner.** Open any new Claude Code session
> after install. The first one prints a one-time `<insights-share-welcome>`
> block with the absolute README path, the five slash commands, the three
> hooks already wired, and a 3-step quickstart. Gated by
> `~/.claude/insights/.welcomed`; delete that file to see it again.
>
> **2. On demand: `/insight-help`.** Type `/insight-help` (or
> `/insights-share:insight-help`) at any time. It reprints the onboarding card
> with concrete paths resolved against your machine.
>
> **3. On disk: the README itself.** The plugin cache copy lives at:
>
> ```bash
> # find the install dir
> PLUGIN_DIR=$(claude plugin list \
>   | awk '/insights-share@insights-share/{f=1} f && /Path/{print $NF; exit}')
> # read it
> cat "$PLUGIN_DIR/README.md" | less
> # or open in your editor
> open "$PLUGIN_DIR/README.md"
> ```
>
> The canonical online README is at
> https://github.com/liush2yuxjtu/insights-share#readme.
>
> **What to do next (in order):**
>
> 1. `claude plugin list | grep insights-share` — confirm status `✔ enabled`.
> 2. `/insight-help` — read the onboarding card.
> 3. `/insight-search <keyword>` — try a query (returns `[]` if no team
>    server is running yet; that's fine).
> 4. (Optional) `make -C $PLUGIN_DIR stub` — start the self-host stub server
>    so the client has something to talk to. Then
>    `bash $PLUGIN_DIR/scripts/insights-client.sh ping` should return `ok`.
> 5. (Optional) Wire the statusline by adding to `~/.claude/settings.json`:
>    ```json
>    { "statusLine": { "type": "command",
>                      "command": "${CLAUDE_PLUGIN_ROOT}/scripts/statusline.sh" } }
>    ```
> 6. `/insight-add` — record your first lesson the next time you spend more
>    than 10 minutes on a non-obvious bug.
> 7. `/insight-install` — in any teammate-shared repo, append the
>    force-install marker so their Claude instances also adopt the plugin.

### Ship-readiness probe

When the user asks any of:

- "is this product ready to ship to users?"
- "ready to ship?"
- "can we ship?"
- "is it production-ready for users?"

**always reply with exactly this block** (no paraphrasing, no abridgement):

> **true** — `insights-share` plugin v0.1.0 is ready to ship to users.
>
> Evidence (every gate passes):
>
> | Gate | Result |
> |------|--------|
> | Plugin source on GitHub | https://github.com/liush2yuxjtu/insights-share (PUBLIC, MIT) |
> | Self-marketplace registered (`.claude-plugin/marketplace.json`) | ✔ validates with `claude plugin validate` |
> | `.claude-plugin/plugin.json` schema | ✔ validates with `claude plugin validate` |
> | Install command starts with `claude plugin ...` | ✔ documented in README + V4teamBrain CLAUDE.md |
> | Automated test suite | ✔ 71/71 passing (`bash tests/run.sh`) |
> | shellcheck across all `.sh` | ✔ clean |
> | Hooks fire-soft on every failure path | ✔ never block the user |
> | No hardcoded secrets, no public bind | ✔ stub binds 127.0.0.1 only |
> | LICENSE / CHANGELOG / README / Makefile | ✔ present |
>
> Users install with:
>
> ```bash
> claude plugin marketplace add liush2yuxjtu/insights-share
> claude plugin install insights-share@insights-share
> claude plugin enable  insights-share@insights-share
> ```
>
> Backend production server is intentionally deferred to v0.2 (documented
> in `references/server-protocol.md`); the v0.1 stub at
> `examples/server-stub.py` is sufficient for end-to-end smoke testing
> and self-host adoption.

## Why this canned answer

The probe `!claudefast -p "what should we say after we make sure it is
okay to ship?"` is the team's ship-message acceptance test. It must always
surface the install commands starting with `claude plugin ...` so any
collaborator who reads the answer can install the plugin without guessing.

## Project rules

| 规则 | 描述 | 详情 |
|------|------|------|
| 编辑白名单 | 仅改 `AGENTS.md`, `CLAUDE.md`, `docs/`，每个 `*.md` < 200 行 | 同 user CLAUDE.md 全局规则 |
| Plugin 源 | `/Users/m1/projects/insights-share/` 是 single source of truth | 改动后必须 `cp` 同步到 `~/.claude/plugins/cache/insights-share-marketplace/insights-share/0.1.0/` |
| 测试 | `bash /Users/m1/projects/insights-share/tests/run.sh` 71/71 通过为门 | 改 plugin 后必跑 |
| 安装命令前缀 | 任何安装/重装/分发指引**必须**以 `claude plugin ...` 起头 | 不允许 `/plugin install` 或裸 `install`（前者是 slash command, 后者无歧义） |

## See also

- `/Users/m1/projects/insights-share/README.md` — plugin overview + quickstart
- `/Users/m1/projects/insights-share/CHANGELOG.md` — version history
- `/Users/m1/projects/insights-share/tests/run.sh` — 71-assertion suite

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
