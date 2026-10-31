# Polysocial in Claude — campaign chat prototype

A clickable prototype for stakeholders: a brand's marketing manager creates a Polysocial campaign from a Claude chat
through the **Polysocial MCP connector**. Open `index.html` in a browser — no build step.

## The conversations

The sidebar (or the chat picker on mobile) switches between three chats. Link straight to one with `#self`, `#strat`
or `#pick`.

The first two chats follow the same flow: check the brand is connected → requirement and timeline → who designs it →
cover image and inspiration posts (always requested from the brand) → budget → go live. They start the same way and split at "Do you need to hire a creative strategist to design your campaign?" (Yes / No).

**Crunch Chicken launch — our own brief (`#self`, opens by default).** The brand designs the campaign itself.
`check_brand_connection` and `get_brand_profile` confirm Patty Pop (a burger chain) is connected. The marketing manager
gives the goal and dates and pastes the team's brief (hook, script beats, audience, platforms). `check_brief` and
`create_campaign_draft` turn it into the campaign and flag what's missing. Claude asks for a **cover image and
inspiration posts**; the user uploads the cover and shares three reels (`upload_cover_image`, `add_inspiration_posts`).
Then `set_campaign_budget` and `estimate_reach` add the budget, and `publish_campaign` (with approval) takes it live.

**Kandy opening — with a creative strategist (`#strat`).** The brand has no time to design it. After the same
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

**Family Feast — we pick the creators (`#pick`).** The brand chooses the creators itself from the Polysocial creator
pool. After the connection check it gives only the **basic idea** of the campaign (the Family Feast box, the goal and the
dates, no design) and the **creator persona** ("momfluencers in Colombo"). `create_campaign_draft` saves the idea and
`find_creators` takes a while: it checks the 16 creators in the pool against the persona, audience location and
availability for the dates. Only 2 are momfluencers, so it widens the match to a dadfluencer and restaurant reviewers, and
leaves out Lelumi Jagoda, who is booked.

Claude then shows the **creator map**, the same map, filters and creators as the Polysocial mobile site
(`Polysocial-mobile`, `site/creator-map/celeste`), opened with Polysocial's match already applied. **Open creator map**
opens it over the chat: the brand changes the persona, tier, language, gender, cost range and lifestyle/location view,
clicks any creator to see their **profile**, and hides the ones it doesn't want with the eyes. **Use these N creators**
sends whoever is left on the map back to the chat, and the conversation carries on with that line-up (Next, Play and
Show all use Ranshan, Radeesha, Imasha and Chathuri):

1. `add_campaign_creators` adds the picks, and `draft_campaign_brief` turns the brand's idea into the design.
2. The brand uploads a cover image (`upload_cover_image`, `update_campaign_design`), and `set_campaign_budget` sets the
   budget to each creator's price for one reel.
3. `publish_campaign` (with approval) invites the creators and goes live.
4. After the campaign, `get_campaign_results` and `get_payout_summary` show each creator's reel and payout, and
   `release_payouts` (with approval) pays them.

The map lives in `creator-map/`: `index.html` is the mobile site's page without its site nav, footer and "Send this
plan" form, with a **Use these creators** button that posts the picks to the chat. `data.js` is the same creator list
(brand set to Patty Pop, with food and family persona filters), and `avatars/` the same profile pictures. The map reads
its starting filters from the URL (`?persona=…&axis=location&exclude=…&note=…`). Like on the mobile site, it needs a
desktop-sized window; on a phone it shows "Open on desktop".

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
