# Polysocial in Claude — campaign chat prototype

A clickable prototype for stakeholders: a brand's marketing manager runs Polysocial campaigns from a Claude chat
through the **Polysocial MCP connector**. Open `index.html` in a browser — no build step.

## The conversations

The sidebar (or the chat picker on mobile) switches between four chats. Link straight to one with `#self`, `#strat`,
`#avurudu` or `#mocha`.

### Campaign creation flow — two paths

Both chats follow the same flow: check the brand is connected → requirement and timeline → who designs it →
cover image and inspiration posts (always requested from the brand) → budget → go live. They start the same way and split at "Do you need to hire a creative strategist to design your campaign?" (Yes / No).

**Cold Brew launch — our own brief (`#self`, opens by default).** The brand designs the campaign itself.
`check_brand_connection` and `get_brand_profile` confirm Starbucks Sri Lanka is connected. The marketing manager
gives the goal and dates and pastes the team's brief (hook, script beats, audience, platforms). `check_brief` and
`create_campaign_draft` turn it into the campaign and flag what's missing. Claude asks for a **cover image and
inspiration posts**; the user uploads the cover and shares three reels (`upload_cover_image`, `add_inspiration_posts`).
Then `set_campaign_budget` and `estimate_reach` add the budget, and `publish_campaign` (with approval) takes it live.

**Kandy store opening — with a strategist (`#strat`).** The brand has no time to design it. After the same
connection check, `get_strategist_options` explains the service, and the budget is set first because a strategist
can't be requested without one. Claude then asks for a **cover image and inspiration posts** (`upload_cover_image`, `add_inspiration_posts`),
which are shared with the strategist as the visual direction. `request_creative_strategist` (with approval) assigns
Ishara Fernando. Two days later
`get_strategist_design` returns the design, `send_strategist_feedback` gets a revised version 2, and
`approve_strategist_design` + `publish_campaign` take it live, with the strategist running it.

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
