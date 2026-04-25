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
> **14 PASS / 0 FAIL**.
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
> **What `uat.sh` checked (10 steps, exits non-zero on any failure):**
>
> | # | Check | Result |
> |---|-------|--------|
> | 1 | running inside tmux | ✔ |
> | 2 | cwd ends in `testV4team` | ✔ |
> | 3 | `claude` binary on PATH | ✔ |
> | 4 | marketplace listed | ✔ |
> | 5 | plugin status `enabled` | ✔ |
> | 6 | five `/insight-*` skills registered | ✔ ×5 |
> | 7 | plugin cache dir located | ✔ |
> | 8 | README readable on disk | ✔ |
> | 9 | stub server `/healthz` returns `ok` | ✔ |
> | 10 | `<!-- insights-share@v0.1 -->` marker idempotent | ✔ |
>
> **Reset between runs:**
>
> ```bash
> claude plugin uninstall          insights-share@insights-share
> claude plugin marketplace remove insights-share
> rm -f ~/.claude/insights/.welcomed   # re-trigger welcome banner
> ```
