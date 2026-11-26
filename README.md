# Nookal API integration and MCP server: connect Nookal to ChatGPT, Claude and any AI agent

**Nookal API integration and MCP server by Cotlet.dev: yes, Claude, ChatGPT and other AI agents can work in your Nookal account.** Nookal has no official MCP server and no ChatGPT or Claude app, and no public API documentation was found (checked 26 Nov 2026). Cotlet.dev connects your own signed-in Nookal account to any AI agent, so you ask in plain words and the AI helps you chase ageing debts, fill gaps, win back lapsed patients and watch NDIS budgets.

## What you can ask

> There is a gap on Thursday at 2pm. Who on the waiting list wants an earlier slot with that practitioner? Draft an SMS to them.

> From the Last Attendances report, list patients last seen 6 to 12 weeks ago with no booking. Draft a check-in message for each.

> Which balances are over 30 days in the Ageing Debts report? Split patients from NDIS, TAC and Workcover payers and draft reminders.

> Which NDIS clients have less than 20% of their plan budget left or a plan ending this month?

> Run the Clinical Notes report for last week. Which attended appointments still have no note, per practitioner?

## What physio, chiro and allied health clinic owners and receptionists ask their AI to do in Nookal

From physio, chiro and allied health clinic owners and receptionists' posts on Reddit, forums, review sites and the Nookal help center (checked 26 Nov 2026).

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Fill gaps from the waiting list | "There is a gap on Thursday at 2pm. Who on the waiting list wants an earlier slot with that practitioner? Draft an SMS to them." | When you ask |
| Reactivate lapsed patients | "From the Last Attendances report, list patients last seen 6 to 12 weeks ago with no booking. Draft a check-in message for each." | Weekly |
| Chase aged debts and third party invoices | "Which balances are over 30 days in the Ageing Debts report? Split patients from NDIS, TAC and Workcover payers and draft reminders." | Weekly |
| NDIS plan budgets running low | "Which NDIS clients have less than 20% of their plan budget left or a plan ending this month?" | Weekly |
| Appointments without clinical notes | "Run the Clinical Notes report for last week. Which attended appointments still have no note, per practitioner?" | Weekly |
| Unconfirmed appointments for tomorrow | "Which of tomorrow's appointments are still unconfirmed? List phone numbers so reception can call." | Daily |
| Weekly practitioner KPIs | "Give me last week's utilisation, new patients and rebook rate per practitioner from the Business Performance report." | Weekly |
| Client data to a sheet or CRM | "Export this month's new clients with name, email and referral source into my sheet." | Your own endpoint, called from a sheet or n8n |

Your AI asks you before it sends, books, replies or changes anything. Payments stay in your hands.

## Nookal API

Nookal lets a clinic generate an API key in Setup > Connections > API Keys, but no public API documentation was found, so a developer has to get it from Nookal first (checked 26 Nov 2026). With Cotlet.dev you build your own Nookal API: show your AI agent a job once in your signed-in Nookal account, save it as an endpoint and call it from n8n, Make, Zapier or your own code. How it works: https://cotlet.dev/integrations/api/nookal

Endpoints you can build in a few minutes (none of them exists until you build it):

- `new-clients`: New clients in a date range with email and referral source
- `ageing-debts`: Balances over N days by payer
- `ndis-budgets`: NDIS clients with budget left and plan end date
- `unconfirmed-tomorrow`: Tomorrow's appointments still unconfirmed, with phone numbers
- `lapsed-clients`: Clients last seen 6 to 12 weeks ago with no booking
- `book-appointment`: Book a client into a free slot and return the time

Call one with your Cotlet API key: `POST https://cotlet.dev/api/v1/custom/new-clients`. One call runs up to 90 seconds, writes wait for your OK, payments are blocked.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No API key needed.

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Nookal tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"nookal":{"command":"uvx","args":["nookal-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t nookal-mcp .
docker run -i --rm -e COTLET_API_KEY nookal-mcp
```

Tools: `nookal_guide`, `nookal_status`, `nookal_open`, `nookal_read`, `nookal_click`, `nookal_type`, `nookal_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install nookal-mcp        # or run it once: uvx nookal-mcp connect
nookal-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 nookal_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 nookal_agent.py status    # is Nookal ready in your Cotlet.dev account
python3 nookal_agent.py guide     # Nookal screens and sign-in link
python3 nookal_agent.py read      # open Nookal, read the page, close the browser
python3 nookal_agent.py tools     # list Cotlet.dev tools
python3 nookal_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Nookal users

**Does Nookal have an API?**

Nookal lets a clinic generate an API key in Setup > Connections > API Keys, but no public API documentation was found, so a developer has to get it from Nookal first (checked 26 Nov 2026). With Cotlet.dev you build your own Nookal API on your signed-in account and call it from n8n, Make or your own code: https://cotlet.dev/integrations/api/nookal

**How do I connect Nookal to ChatGPT?**

Add Cotlet.dev as an app in ChatGPT (Settings → Apps, connector URL `https://cotlet.dev/mcp/v1`), sign in to Nookal once in your Cotlet.dev Cloud Browser, and ask ChatGPT in plain words. No code and no Nookal API key.

**Is there a way to use Claude with Nookal?**

Yes. In Claude Code run the one-line `claude mcp add` command above; in the Claude app add `https://cotlet.dev/mcp/v1` as a custom connector. Claude then works in your own Nookal account.

**Is there a Nookal MCP server?**

Nookal has no official MCP server for your account. Cotlet.dev is a hosted MCP server that works in your own Nookal account for you, with Claude, ChatGPT, Cursor, Codex, n8n, Make and Zapier.

**Can I get Nookal data into Google Sheets or my CRM without the API documentation?**

Yes. Your AI opens the report or client list in your signed-in Nookal account, reads it and fills your sheet. Nookal shows no public API documentation (checked 26 Nov 2026) and has no Zapier app, so this skips both.

**How is Cotlet.dev different from Nookal's JAN assistant?**

JAN works inside Nookal, in its own chat. Cotlet.dev lets ChatGPT, Claude or n8n work in your Nookal account, on a schedule, and combine it with your sheets, email and other apps.

## Find it on

- [Nookal on Cotlet.dev](https://cotlet.dev/integrations/mcp/nookal?utm_source=github&utm_medium=readme&utm_campaign=nookal&utm_content=find)
- [PyPI package nookal-mcp](https://pypi.org/project/nookal-mcp/)
- [Odel](https://odel.app/modules/cotlet/nookal)
- [mcpbeat](https://mcpbeat.com/mcp-servers/cotlet/nookal/)

## Also works with

- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
- [Wanderlog MCP](https://github.com/cotlet-connect/wanderlog-mcp)
- [SmugMug MCP](https://github.com/cotlet-connect/smugmug-mcp)
- [Garmin Connect MCP](https://github.com/cotlet-connect/garmin-mcp)
- [Housecall Pro MCP](https://github.com/cotlet-connect/housecall-pro-mcp)
- [Medium MCP](https://github.com/cotlet-connect/medium-mcp)
- [Crexi MCP](https://github.com/cotlet-connect/crexi-mcp)
- [DistroKid MCP](https://github.com/cotlet-connect/distrokid-mcp)
- [LinkedIn MCP](https://github.com/cotlet-connect/linkedin-mcp)
- [EstimateOne MCP](https://github.com/cotlet-connect/estimateone-mcp)
- [Google Search Console MCP](https://github.com/cotlet-connect/google-search-console-mcp)
- [Lichess MCP](https://github.com/cotlet-connect/lichess-mcp)
- [Strava MCP](https://github.com/cotlet-connect/strava-mcp)
- [StreamElements MCP](https://github.com/cotlet-connect/streamelements-mcp)
- [Meetup MCP](https://github.com/cotlet-connect/meetup-mcp)
- [AccuRanker MCP](https://github.com/cotlet-connect/accuranker-mcp)
- [GetYourGuide MCP](https://github.com/cotlet-connect/getyourguide-mcp)
- [Idealista MCP](https://github.com/cotlet-connect/idealista-mcp)
- [Riverside MCP](https://github.com/cotlet-connect/riverside-mcp)
- [Udemy MCP](https://github.com/cotlet-connect/udemy-mcp)
- [Faire MCP](https://github.com/cotlet-connect/faire-mcp)
- [Teachable MCP](https://github.com/cotlet-connect/teachable-mcp)
- [Vagaro MCP](https://github.com/cotlet-connect/vagaro-mcp)
- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
- [Zoopla MCP](https://github.com/cotlet-connect/zoopla-mcp)
- [Dribbble MCP](https://github.com/cotlet-connect/dribbble-mcp)
- [Hostelworld MCP](https://github.com/cotlet-connect/hostelworld-mcp)
- [Mashvisor MCP](https://github.com/cotlet-connect/mashvisor-mcp)
- [Rent Manager MCP](https://github.com/cotlet-connect/rent-manager-mcp)
- [ThriveCart MCP](https://github.com/cotlet-connect/thrivecart-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [Booksy MCP](https://github.com/cotlet-connect/booksy-mcp)
- [BookingKoala MCP](https://github.com/cotlet-connect/bookingkoala-mcp)
- [Milanote MCP](https://github.com/cotlet-connect/milanote-mcp)
- [Partiful MCP](https://github.com/cotlet-connect/partiful-mcp)
- [Contractor Foreman MCP](https://github.com/cotlet-connect/contractor-foreman-mcp)
- [Pixieset MCP](https://github.com/cotlet-connect/pixieset-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [ZenMaid MCP](https://github.com/cotlet-connect/zenmaid-mcp)
- [AroFlo MCP](https://github.com/cotlet-connect/aroflo-mcp)
- [Simpro MCP](https://github.com/cotlet-connect/simpro-mcp)
- [Workiz MCP](https://github.com/cotlet-connect/workiz-mcp)
- [Kickserv MCP](https://github.com/cotlet-connect/kickserv-mcp)
- [Service Autopilot MCP](https://github.com/cotlet-connect/service-autopilot-mcp)
- [Mindbody MCP](https://github.com/cotlet-connect/mindbody-mcp)
- [DaySmart MCP](https://github.com/cotlet-connect/daysmart-mcp)
- [Acuity Scheduling MCP](https://github.com/cotlet-connect/acuity-scheduling-mcp)
- [Jane App MCP](https://github.com/cotlet-connect/jane-app-mcp)
- [FieldEdge MCP](https://github.com/cotlet-connect/fieldedge-mcp)
- [Phorest MCP](https://github.com/cotlet-connect/phorest-mcp)
- [DINGG MCP](https://github.com/cotlet-connect/dingg-mcp)
- [Invoice Simple MCP](https://github.com/cotlet-connect/invoice-simple-mcp)
- [PhotoShelter MCP](https://github.com/cotlet-connect/photoshelter-mcp)
- [RazorSync MCP](https://github.com/cotlet-connect/razorsync-mcp)
- [ShootProof MCP](https://github.com/cotlet-connect/shootproof-mcp)
- [Invoice2go MCP](https://github.com/cotlet-connect/invoice2go-mcp)
- [Appointy MCP](https://github.com/cotlet-connect/appointy-mcp)
- [Contractor+ MCP](https://github.com/cotlet-connect/contractor-plus-mcp)
- [Petpooja MCP](https://github.com/cotlet-connect/petpooja-mcp)
- [Teachers Pay Teachers MCP](https://github.com/cotlet-connect/teachers-pay-teachers-mcp)
- [Bloom MCP](https://github.com/cotlet-connect/bloom-mcp)
- [hipages MCP](https://github.com/cotlet-connect/hipages-mcp)
- [ServiceSeeking MCP](https://github.com/cotlet-connect/serviceseeking-mcp)
- [MyJobQuote MCP](https://github.com/cotlet-connect/myjobquote-mcp)
- [MyBuilder MCP](https://github.com/cotlet-connect/mybuilder-mcp)
- [Yelp for Business MCP](https://github.com/cotlet-connect/yelp-for-business-mcp)
- [Rosie MCP](https://github.com/cotlet-connect/rosie-mcp)
- [Roofr MCP](https://github.com/cotlet-connect/roofr-mcp)
- [FieldPulse MCP](https://github.com/cotlet-connect/fieldpulse-mcp)
- [YourTradebase MCP](https://github.com/cotlet-connect/yourtradebase-mcp)
- [JobNimbus MCP](https://github.com/cotlet-connect/jobnimbus-mcp)
- [Ascora MCP](https://github.com/cotlet-connect/ascora-mcp)
- [eLocal MCP](https://github.com/cotlet-connect/elocal-mcp)
- [Alto MCP](https://github.com/cotlet-connect/alto-mcp)
- [Rightmove Plus MCP](https://github.com/cotlet-connect/rightmove-plus-mcp)
- [TurboTenant MCP](https://github.com/cotlet-connect/turbotenant-mcp)
- [Innago MCP](https://github.com/cotlet-connect/innago-mcp)
- [Property Finder MCP](https://github.com/cotlet-connect/property-finder-mcp)
- [LoopNet MCP](https://github.com/cotlet-connect/loopnet-mcp)
- [Dotloop MCP](https://github.com/cotlet-connect/dotloop-mcp)
- [SkySlope MCP](https://github.com/cotlet-connect/skyslope-mcp)
- [ShowingTime MCP](https://github.com/cotlet-connect/showingtime-mcp)
- [connectMLS MCP](https://github.com/cotlet-connect/connectmls-mcp)
- [WebPT MCP](https://github.com/cotlet-connect/webpt-mcp)
- [Tebra MCP](https://github.com/cotlet-connect/tebra-mcp)
- [ChiroTouch MCP](https://github.com/cotlet-connect/chirotouch-mcp)
- [ChiroFusion MCP](https://github.com/cotlet-connect/chirofusion-mcp)
- [Semble MCP](https://github.com/cotlet-connect/semble-mcp)
- [Cliniko MCP](https://github.com/cotlet-connect/cliniko-mcp)
- [Gentu MCP](https://github.com/cotlet-connect/gentu-mcp)
- [splose MCP](https://github.com/cotlet-connect/splose-mcp)
- [Physitrack MCP](https://github.com/cotlet-connect/physitrack-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/mcp/nookal?utm_source=github&utm_medium=readme&utm_campaign=nookal

Start free: https://cotlet.dev/signup?app=nookal&utm_source=github&utm_medium=readme&utm_campaign=nookal
