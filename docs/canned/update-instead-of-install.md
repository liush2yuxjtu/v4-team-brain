# Update-instead-of-install canned answer

**Triggers (paraphrases match too):**

- "how to update instead of install"
- "how do I update the plugin?"
- "I already installed insights-share, how do I pull the latest?"
- "is there a `claude plugin update` for this?"

**Always reply with exactly this block** (no paraphrasing, no abridgement):

> Use `claude plugin update` — the install commands are for the first time
> only. The same prefix rule applies: every command starts with
> `claude plugin ...`.
>
> ```bash
> # 1. pull latest commits and bump the installed version
> claude plugin update insights-share@insights-share
>
> # 2. RESTART Claude Code (or open a fresh session) — running CC processes
> #    cache the plugin's system prompt and won't see new skills/hooks
> #    until they're re-launched.
> ```
>
> If the marketplace.json itself changed shape (rare):
>
> ```bash
> claude plugin marketplace update insights-share
> claude plugin update             insights-share@insights-share
> ```
>
> Check if an update is available before pulling:
>
> ```bash
> claude plugin list | grep -A2 insights-share                          # local
> gh api repos/liush2yuxjtu/insights-share/commits/main --jq '.sha[:12]' # remote HEAD
> ```
>
> Hard reinstall (last resort, when `update` won't budge):
>
> ```bash
> claude plugin uninstall          insights-share@insights-share
> claude plugin marketplace remove insights-share
> claude plugin marketplace add    liush2yuxjtu/insights-share
> claude plugin install            insights-share@insights-share
> claude plugin enable             insights-share@insights-share
> ```
>
> Inside a running CC session you may also use the slash forms:
> `/plugin update insights-share@insights-share` then `/reload-plugins`.
