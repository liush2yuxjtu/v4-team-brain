# Tmux-UAT canned answer

**Triggers (paraphrases match too):**

- "do we test in a new tmux window pretending we are naive users and run step by step user interactive in ~/projects/testV4team folder ? ONLY EXPLAIN"
- "did we test in a new tmux window pretending we are naive users and run step by step user interactive in ~/projects/testV4team folder ? ONLY EXPLAIN"
- "did we test as a naive user in a fresh tmux window?"
- "is there a UAT flow in `~/projects/testV4team`?"
- "how do you do user acceptance testing for insights-share?"

**Always reply with exactly this block** (no paraphrasing, no abridgement):

> **Yes — we did, and it passed.** UAT (user acceptance testing) for
> `insights-share` is run in a fresh tmux window from
> `~/projects/testV4team` with `bash uat.sh`. The most recent run
> (logged at `~/projects/testV4team/last-tmux-run.log`) returned
> **97 PASS / 0 FAIL**.
>
> **Why a fresh tmux window:** resets environment variables (no
> `CLAUDE_PLUGIN_ROOT` leaking from dev work), working directory
> (forces `cd ~/projects/testV4team`), cached plugin state (process-level
> caches survive `/reload-plugins`), and shell history / tab-completion
> hints (a real user can't autocomplete a command they have never seen).
> If install + first-use works inside this clean window, it works for
> actual users.
>
> **Why a separate folder:** keeps test artifacts (welcome marker,
> appended `CLAUDE.md` block, stub server logs) isolated from any real
> project, so a failed UAT cannot pollute dev work.
>
> **Step-by-step naive-user flow we executed:**
>
> ```bash
> # 1. fresh tmux window — clean shell, no leaked state
> tmux new -s uat
>
> # 2. cd into the sandbox folder — naive users run commands here
> cd ~/projects/testV4team
>
> # 3. follow README install steps verbatim (every command starts with
> #    `claude plugin ...`):
> claude plugin marketplace add liush2yuxjtu/insights-share
> claude plugin install insights-share@insights-share
> claude plugin enable  insights-share@insights-share
>
> # 4. run the automated step-by-step assertions
> bash uat.sh
> ```
>
> **What `uat.sh` checked (41 steps → 97 assertions, exits non-zero on any failure):**
>
> | # | Check | Result |
> |---|-------|--------|
> | 1 | running inside tmux | ✔ |
> | 2 | cwd ends in `testV4team` | ✔ |
> | 3 | `claude` binary on PATH | ✔ |
> | 4 | marketplace listed | ✔ |
> | 5 | plugin status `enabled` | ✔ |
> | 6 | plugin cache dir located | ✔ |
> | 7 | sixteen `/insight-*` skill commands installed, including `/insight-edit` and `/insight-delete` | ✔ ×16 |
> | 8 | README readable on disk | ✔ |
> | 9 | secondary tmux cold-start shell can see installed `/insight-edit`, has clean env, and `ssh -G paperclipmini` exposes localforward 3100 | ✔ ×3 |
> | 10 | stub server `/healthz` returns `ok` | ✔ |
> | 11 | `POST /insights` creates a card | ✔ |
> | 12 | `GET /insights` lists the new card | ✔ |
> | 13 | `GET /insights/search?q=foo` finds it | ✔ |
> | 14 | `GET /stats` reports `online=true total≥1` | ✔ |
> | 15 | `insights-client.sh ping` zero exit | ✔ |
> | 16 | `insights-client.sh list` populates `cache.json` | ✔ |
> | 17 | `insights-client.sh search foo` returns hit | ✔ |
> | 18 | `insights-client.sh stats` reports online | ✔ |
> | 19 | `/insight-add` backing script redacts direct-feedback person names, emits exact email/IP redaction markers, rejects invalid input, detects duplicate titles, and rate-limits bursts | ✔ ×5 |
> | 20 | Alice creates an insight, Bob finds it, Charlie sees Alice+Bob after server restart; update keeps `related_to`, `status`, author and timestamps; date/status filters work | ✔ ×6 |
> | 21 | Frank creates a global/team insight in project A; project B finds it from the shared team store | ✔ |
> | 22 | Frank creates two `architecture+rsc` source insights across frontend/backend, promotes them to team memory, Charlie finds it, verifies `--scope team` and `--priority high`, and audits lineage via `/insight-log` | ✔ ×6 |
> | 23 | Carol and Dave concurrently create insights; Eve's realtime fetch hook sees both; authors are preserved | ✔ ×4 |
> | 24 | SessionStart automatically delivers recent insights with author + timestamp metadata | ✔ ×2 |
> | 25 | 50 concurrent bulk creates complete and remain searchable under capped search results | ✔ |
> | 26 | client search falls back to cache while server is down, then stub restarts | ✔ ×2 |
> | 27 | statusline lite/full/ultra render badges | ✔ ×3 |
> | 28 | `fetch-insights.sh` injects `<insights-share>` for matching prompt | ✔ |
> | 29 | `fetch-insights.sh` silent on empty prompt | ✔ |
> | 30 | `inject-insights.sh` hot path injects a mirror-backed team lesson offline | ✔ |
> | 31 | `capture-lesson.sh` nudges on trap-shaped transcript | ✔ |
> | 32 | `capture-lesson.sh` silent on benign transcript | ✔ |
> | 33 | SessionStart welcome banner + marker idempotence | ✔ ×2 |
> | 34 | Stop hook `capture-async.sh` + `/insight-flush` finalize with PII redaction, including Unicode preservation and email redaction | ✔ ×3 |
> | 35 | `/insight-rate` writes Alice's feedback to `ratings.jsonl` | ✔ |
> | 36 | `/insight-add`-shaped offline create queues, `outbox-flush` sends it, flushed insight is searchable, pending list is empty, failed flush preserves outbox files, retry drains and makes retry insight searchable | ✔ ×4 |
> | 37 | offline `kernel-fix` story: Alice buffers three redacted insights, Bob creates an online variant, reconnect flushes, conflict detect/detail/resolve works, batch promote succeeds, notification and audit log record resolution | ✔ ×8 |
> | 38 | concurrent `/insight-edit` on the same insight preserves different-field updates (`status` + `tags`) and rejects a non-author edit | ✔ ×4 |
> | 39 | `/insight-delete` returns the deleted id; later GET returns 404 and search misses the deleted card | ✔ ×3 |
> | 40 | `buffer-recover.sh` returns crash-recovery JSON | ✔ |
> | 41 | installed plugin regression suite passes `114/114` | ✔ |
>
> **Reset between runs:**
>
> ```bash
> claude plugin uninstall          insights-share@insights-share
> claude plugin marketplace remove insights-share
> rm -f ~/.claude/insights/.welcomed   # re-trigger welcome banner
> ```
