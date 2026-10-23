# Postkey MCP server: Instagram DM automation for AI agents

[Postkey](https://postkey.pro) (postkey.pro) is agentic Instagram DM automation and post scheduling. Connect your AI
agent (Claude, ChatGPT, Cursor, Grok Bot, OpenClaw, Hermes Agent or any MCP client) and it can build comment-to-DM
automations, send a link by DM when someone comments a keyword, gate the link behind a follow (follow to unlock),
collect emails, answer your Instagram inbox and schedule posts. People use it as a ManyChat alternative for the part
an agent does: the agent builds, edits and switches on the automations itself.

```
https://postkey.pro/mcp
```

Remote MCP server, Streamable HTTP, OAuth 2.1 sign-in (dynamic client registration, PKCE). Nothing to install. This
repository holds no code: it is the public home of the listing (`server.json` is the entry in the official MCP
Registry, `pro.postkey/postkey`).

## Connect

You need a Postkey profile with an Instagram account connected. Most assistants send you to
Postkey to sign in, pick the profile, and choose what they may do; Cursor and scripts use an agent key made in
Postkey instead.

| Client | Setup |
|---|---|
| Claude (web, desktop, phone, Claude Code) | Customize, then Connectors, then "Add custom connector", and paste `https://postkey.pro/mcp`. |
| ChatGPT (Plus and above; Codex too) | Open [chatgpt.com/plugins](https://chatgpt.com/plugins), press the plus button, choose "Add custom MCP server", and paste `https://postkey.pro/mcp`. |
| Grok Bot | On grok.com: Plugins, then Connectors, then "New Connector", then "Custom"; paste `https://postkey.pro/mcp` under Server URL. |
| Cursor | Make an agent key in Postkey (Agent & API), then add to `~/.cursor/mcp.json`: `{"mcpServers":{"postkey":{"url":"https://postkey.pro/mcp","headers":{"Authorization":"Bearer YOUR_AGENT_KEY"}}}}` |
| OpenClaw | `openclaw mcp add postkey --url https://postkey.pro/mcp --transport streamable-http`, then `openclaw mcp login postkey`. |
| Hermes Agent | `hermes mcp add postkey --url https://postkey.pro/mcp --auth oauth` |
| VS Code (sign-in not confirmed yet) | `.vscode/mcp.json`: `{"servers":{"postkey":{"type":"http","url":"https://postkey.pro/mcp"}}}`, then start the server. |
| Any other MCP client | Give it `https://postkey.pro/mcp`. A script or a client that only takes a header can send an agent key as `Authorization: Bearer <key>`. |

Full steps and screenshots: [postkey.pro/docs/mcp](https://postkey.pro/docs/mcp).

## What your agent can do

45 tools, all named `postkey_*`. A connection sees only the ones its sign-in allowed.

- **DM automations (Instagram):** list templates (comment to link, follow to unlock, collect emails, story replies,
  DM keyword replies and more), create one as a draft, change its keywords, link and messages (automations made with the Quick form; Flow Builder
  ones can only be renamed), turn it on, pause it,
  and see how many people started it, how many DMs went out and how many links were tapped. DM sequences and their
  subscribers.
- **Inbox:** list and read Instagram conversations, reply, assign, label and add internal notes.
- **Contacts:** search, read and tag.
- **Posts and scheduling:** draft posts, attach files from the Library (or upload one), schedule or publish to the
  connected accounts (Instagram, TikTok, Facebook, YouTube, LinkedIn, Threads, Pinterest, Bluesky, Telegram, Google
  Business), unschedule, and see how published posts did.
- **Start and safety:** `postkey_get_started` (the profile, its limits and what is set up), `postkey_guide`,
  `postkey_preview_action` (shows exactly what a send would do) and what is waiting for approval.
- **X:** a check-before-send toolkit for an agent that posts with your own X access (rules, quiet hours, limits, STOP
  list). Postkey itself only publishes posts you schedule on X.

Postkey also answers Messenger DMs for Facebook Pages; those automations are set up in the Postkey app, and follow to
unlock is Instagram only.

## You stay in control

Turning an automation on, sending a reply and publishing a post wait in Postkey's Approvals until you press Approve
(the profile owner can change this per connection). The agent never sees your passwords, cannot touch billing, your
team or your connected accounts, and text written by other people (DMs, comments) is marked as outside text so the
agent does not follow instructions hidden in it. End a connection any time under Agent & API.

## Example prompts

- "Set up comment-to-DM: when someone comments GUIDE, DM them the link, followers only."
- "Make a follow-to-unlock automation: people who comment LINK get the link once they follow me."
- "Collect emails from comments: when someone comments EBOOK, ask for their email, then send the download link."
- "Which of my DM automations sent the most messages, and are any of them paused?"
- "Show me today's Instagram DMs that nobody has answered yet, and draft a reply to each one for me to approve."
- "Draft an Instagram post for the new drop with the newest photo from my library, and schedule it for Friday at 9am."

## Links

- Website: [postkey.pro](https://postkey.pro)
- MCP docs: [postkey.pro/docs/mcp](https://postkey.pro/docs/mcp)
- Agent skill file: [postkey.pro/skills/postkey/SKILL.md](https://postkey.pro/skills/postkey/SKILL.md)
- Support: [postkey.pro/support](https://postkey.pro/support)
- Privacy: [postkey.pro/privacy](https://postkey.pro/privacy) · Terms: [postkey.pro/terms](https://postkey.pro/terms)
