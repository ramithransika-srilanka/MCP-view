# Polysocial in Claude — campaign chat prototype

A clickable prototype for stakeholders: a brand's marketing manager runs Polysocial campaigns from a Claude chat
through the **Polysocial MCP connector**. Open `index.html` in a browser — no build step.

## The conversations

The sidebar (or the chat picker on mobile) switches between two chats. Link straight to one with `#avurudu` or `#mocha`.

### Avurudu creator campaign (`#avurudu`, opens by default)

Starbucks Sri Lanka has run campaigns on Polysocial before. Its marketing manager asks Claude for a new one for
Avurudu (Sinhala & Tamil New Year, April):

1. **Read the history** — `get_brand_profile`, `list_campaigns`, `get_campaign_results`. Claude compares the four past
   campaigns, pulls out what worked (group content, cheaper creators outside Colombo) and pitches three concepts.
2. **Quick questions** — Claude asks where creators should be based and which product leads; the answers are picked
   as chips.
3. **Draft** — `get_pricing_benchmarks`, `estimate_reach`, `create_campaign_draft`: "Starbucks Avurudu Table",
   Rs. 1.2M, Apr 1–21, island-wide, shown in creator view with a reach estimate.
4. **Early access** — `list_top_creators` and `set_early_access` give the 12 best returning creators 48 hours first.
5. **Publish** — `validate_campaign`, a one-time approval for `publish_campaign`, invites sent and the launch timeline.

### Christmas Mocha review & January launch (`#mocha`)

Dinithi Rajapaksa, Marketing Manager at Starbucks Sri Lanka, talks to Claude in five turns:

1. **Check performance** — Claude calls `list_campaigns` and `get_campaign_performance`, shows budget, creators,
   views by platform and flags the backed-up review queue.
2. **Review submissions** — `list_submissions` + `check_submission_requirements` return the three oldest posts,
   each checked against the campaign's requirements.
3. **Approve and request changes** — a permission prompt for `approve_submission`, two approvals (base payment
   released) and a friendly `request_changes` note for the post missing the brand handle.
4. **Draft a new campaign** — `get_pricing_benchmarks`, `get_brand_profile`, `create_campaign_draft`; the draft is
   shown as creators would see it on the campaign page.
5. **Edit and schedule** — `update_campaign`, then a one-time approval for `schedule_campaign` because it holds the budget.

Each tool call can be expanded to show its request and response.

## Presenting

| Key | Action |
| --- | --- |
| Space / → | Next message |
| P | Auto-play / pause (permission prompts are accepted automatically) |
| A | Show the whole conversation |
| R | Restart |

Campaign, creator and pricing data follow the demo set in the brand web app
(`ramithransika-srilanka/brand-web-app`, commit `61d5f80`). Images in `images/` are taken from that app's `assets.js`.
The tool names are a proposal for the Polysocial MCP server, not a published API.
