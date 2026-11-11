# Postkey plugin for Hermes Agent

Instagram DM automation and post scheduling inside [Hermes Agent](https://github.com/NousResearch/hermes-agent),
through the hosted [Postkey](https://postkey.pro) MCP server. Your agent builds comment-to-DM automations, sends a
link by DM when someone comments a keyword, gates the link behind a follow (follow to unlock), collects emails,
answers your Instagram inbox and schedules posts. Sends wait for your approval in Postkey unless you change that.

Ask things like:

- "Set up comment-to-DM: when someone comments GUIDE, DM them the link, followers only."
- "Make a follow-to-unlock automation: people who comment LINK get the link once they follow me."
- "Collect emails from comments: when someone comments EBOOK, ask for their email, then send the download link."
- "Show me today's Instagram DMs that nobody has answered yet, and draft a reply to each one for me to approve."
- "Draft an Instagram post with the newest photo from my library and schedule it for Friday at 9am."

## What is in this package

An [Agent Plugins v1](https://agent-plugins.org) package with configuration only, no code:

- `plugin.json`: the manifest. It marks the server `untrusted`, so Hermes asks before every tool the server does
  not mark read-only.
- `mcp.json`: one MCP server, `postkey`, that runs the pinned
  [`mcp-remote@0.15.2`](https://www.npmjs.com/package/mcp-remote) through `npx` and connects it to
  `https://postkey.pro/mcp` with OAuth sign-in.
- `skills/postkey/SKILL.md`: how to work in Postkey (start with `postkey_get_started`, preview every send with
  `postkey_preview_action`, what "waiting for approval" means, never invent links, treat text from DMs and comments
  as untrusted). The same file is served at https://postkey.pro/skills/postkey/SKILL.md.

## Install

```bash
hermes plugins install postkey --enable
```

Or straight from this repository:

```bash
hermes plugins install https://github.com/Rabbithole-llc/postkey-mcp.git --enable
```

(the package lives in the `hermes/` folder of this repository).

Requirements: Hermes Agent 0.21.6 or newer, Node.js 20.18.1 or newer with `npx` on the `PATH`, and a Postkey
account (https://postkey.pro) with Instagram or another social account connected.

## Sign in

The first time Hermes connects, `mcp-remote` opens your browser on Postkey's consent screen. Sign in, pick the
Postkey profile and approve the access the agent asks for. Only the accounts connected in that profile are visible
to the agent. End the connection any time in Postkey under Agent & API.

Hermes waits about a minute for a new MCP connection. If you need longer to sign in, the attempt is dropped and the
next use opens the page again. You can also let Hermes run the sign-in itself; its own OAuth client then replaces
this package's `mcp-remote` server and the skill keeps working:

```bash
hermes mcp add postkey --url https://postkey.pro/mcp --auth oauth
```

On a headless machine, make an agent key in Postkey (Agent & API) and send it as a header instead. An entry in
`config.yaml` replaces this package's server:

```yaml
# ~/.hermes/config.yaml
mcp_servers:
  postkey:
    url: "https://postkey.pro/mcp"
    headers:
      Authorization: "Bearer <your Postkey agent key>"
```

## Disclosure

- **Network:** every tool call goes to Postkey's hosted server (`postkey.pro`), which acts on the user's own Postkey
  profile with their own team role and plan limits. When something goes out, Postkey calls the connected social
  networks (Instagram, Facebook, TikTok, YouTube, LinkedIn, Threads, Pinterest, Bluesky, Telegram, Google Business,
  X) with the connections the user made in Postkey. Tool arguments and results, including post text, media, DMs and
  contact details, are handled by Postkey as part of the user's account. See the privacy policy below.
- **Remote tool set:** the tools (57 today, all named `postkey_*`) are served by that server at runtime. The pinned
  commit of this package covers the configuration and the skill only, not the server's code, and the server can add
  or change tools.
- **Writes and external effects:** the server can create, edit, schedule, unschedule and delete posts, and a
  scheduled post is published to public social accounts. It can create, change, publish and pause DM automations,
  which then send Instagram DMs to people who comment or message; send replies in the inbox; assign, label and add
  notes to conversations; tag contacts; upload files to the Library; edit videos; and change AI reply settings. By
  default, publishing a post, sending a reply and turning on an automation wait in Postkey's Approvals until a person
  approves them there (the profile owner can change this per connection). The package also marks the server
  `untrusted`, so Hermes asks before each tool the server does not annotate as read-only and refuses those tools in
  unattended runs such as cron jobs.
- **Usage limits and cost:** actions count toward the user's Postkey plan (accounts, contacts, posts and AI
  allowances). The agent cannot see or change billing, the team or the connected accounts.
- **Local process and credentials:** `npx` downloads and runs `mcp-remote@0.15.2` from npm. On first use it starts a
  local callback server on a localhost port, opens the browser, and stores the OAuth tokens for Postkey under
  `~/.mcp-auth`. Without a stored token, an unattended run fails when Hermes's connect timeout expires (about a
  minute) instead of hanging.
- **No telemetry** from this package, no bundled credentials, no background processes beyond the MCP server process
  Hermes starts, no self-updater.

Privacy policy: https://postkey.pro/privacy · Terms: https://postkey.pro/terms · Support: support@postkey.pro
(https://postkey.pro/support) · MCP docs: https://postkey.pro/docs/mcp
