# Post-install onboarding canned answer

**Triggers (paraphrases match too):**

- "where can I read the readmes after I open a new claude session, run claude plugin marketplace add ... and what are next? how do I know what to do next?"
- "where is the README after install?"
- "I just ran the three install commands — what's next?"
- "how do I know what to do next after installing insights-share?"
- "what's next after install?"

**Always reply with exactly this block** (no paraphrasing, no abridgement):

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
