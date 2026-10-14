# Polysocial in Claude — campaign chat prototype

A clickable prototype for stakeholders: a brand's marketing manager creates a Polysocial campaign from a Claude chat
through the **Polysocial MCP connector**. Open `index.html` in a browser — no build step.

## The conversations

The sidebar (or the chat picker on mobile) switches between two chats. Link straight to one with `#self` or `#strat`.

Both chats follow the same flow: check the brand is connected → requirement and timeline → who designs it →
cover image and inspiration posts (always requested from the brand) → budget → go live. They start the same way and split at "Do you need to hire a creative strategist to design your campaign?" (Yes / No).

**Cold Brew launch — our own brief (`#self`, opens by default).** The brand designs the campaign itself.
`check_brand_connection` and `get_brand_profile` confirm Starbucks Sri Lanka is connected. The marketing manager
gives the goal and dates and pastes the team's brief (hook, script beats, audience, platforms). `check_brief` and
`create_campaign_draft` turn it into the campaign and flag what's missing. Claude asks for a **cover image and
inspiration posts**; the user uploads the cover and shares three reels (`upload_cover_image`, `add_inspiration_posts`).
Then `set_campaign_budget` and `estimate_reach` add the budget, and `publish_campaign` (with approval) takes it live.

**Kandy store opening — with a creative strategist (`#strat`).** The brand has no time to design it. After the same
connection check, `get_strategist_options` explains the service, and the budget is set first (it includes the
strategist's commission). Claude asks for a **cover image and inspiration posts** (`upload_cover_image`,
`add_inspiration_posts`), then `list_creative_strategists` offers **three strategists** to choose from: Ishara Fernando
(best match, 12–15%), Ravindu Perera (cheaper, Colombo-based) and Malsha Gunawardena's agency (full service, 15–18%).

The rest mirrors the **collaborators web app** (`ramithransika-srilanka/collaborators-web-app`), which is the
strategist's side of this flow:

1. `send_strategist_invite` (with approval) sends Ishara an invite. Claude shows it as she sees it in her Invites:
   industry, start date, "One time project · Commission based" tags, the brief and a "Closes in" countdown.
2. When she taps **Start Chat**, Polysocial sends the brand's opening messages for it (summary, files, budget,
   timeline) and asks "What commission would you expect for managing this campaign?"
3. `get_strategist_chat` returns her reply (15%). `get_commission_benchmarks` compares it with her history and
   the category median, and Claude suggests countering at 12%.
4. `send_strategist_message` sends the counter-offer in that same chat. She accepts 12% with weekly reports, and
   `confirm_strategist_commission` (with approval) hires her. The commission is charged only when the design is approved.
5. `get_strategist_design` returns the design, `send_strategist_feedback` gets a revised version 2, and
   `approve_strategist_design` + `publish_campaign` take it live, with the strategist running it.

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
