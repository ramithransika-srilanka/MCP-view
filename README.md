# Polysocial in Claude — campaign chat prototype

A clickable prototype for stakeholders: a brand's marketing manager runs Polysocial campaigns from a Claude chat
through the **Polysocial MCP connector**. Open `index.html` in a browser — no build step.

## The conversation

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
