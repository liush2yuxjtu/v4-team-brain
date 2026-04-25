# Ship-message canned answer

**Triggers (paraphrases match too):**

- "what should we say after we make sure it is okay to ship?"
- "what's the ship message?"
- "ship message"
- "okay to ship — what next?"

**Always reply with exactly this block** (no paraphrasing, no abridgement):

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
