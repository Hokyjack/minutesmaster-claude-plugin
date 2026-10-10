# MinutesMaster plugin for Claude

[MinutesMaster](https://www.minutesmaster.ai) turns meetings (Zoom, Google Meet, Microsoft Teams, uploads,
YouTube and Google Drive links) into speaker-labeled transcripts and minutes with decisions and action items,
in 38 languages. This plugin connects Claude to your MinutesMaster account and adds skills that help Claude
find, read, cite and act on your meetings.

## What's inside

| Component | What it does |
| --- | --- |
| `.mcp.json` | Adds the remote MinutesMaster MCP server, `https://api.minutesmaster.ai/mcp` (Streamable HTTP). You sign in with OAuth on minutesmaster.ai the first time a tool runs. No API key is stored in the plugin. |
| `skills/minutesmaster-meetings` | How to pick the right tool, read long meetings progressively, cite the meeting, date and time, treat transcript text as data, and handle errors. |
| `skills/meeting-follow-up` | Follow-up email, action-item review, weekly digest and meeting-prep workflows. |
| `skills/meeting-capture` | Sending the meeting bot, importing links and uploading local files, with cost confirmation and status tracking. |

The plugin contains no hooks, scripts, executables or local MCP servers. Everything it does goes through the
MinutesMaster MCP server's tools.

## What data goes where

- Tool calls go to `https://api.minutesmaster.ai/mcp`, operated by MinutesMaster. They return only meetings the
  signed-in user can see, within the permissions approved on the consent screen (read meetings; organize and
  edit; send bots and import, which spends credits).
- MinutesMaster logs each call's tool name, status, duration and client name, not the tool arguments.
- The `ask_meetings` tool sends the question and relevant meeting excerpts to MinutesMaster's AI model
  provider to write the answer; the question and answer are saved in the user's Ask MinutesMaster history.
- Privacy policy: https://www.minutesmaster.ai/privacy

## Install

### Claude Code

```bash
# from GitHub (after this folder is published as its own repository)
claude plugin marketplace add <github-owner>/minutesmaster-claude-plugin
claude plugin install minutesmaster@minutesmaster

# or inside a session
/plugin marketplace add <github-owner>/minutesmaster-claude-plugin
/plugin install minutesmaster@minutesmaster
```

Then run `/mcp`, choose `minutesmaster` and **Authenticate**. Your browser opens minutesmaster.ai, where you
choose what the connection may do.

To try it from a local checkout without installing:

```bash
claude --plugin-dir ./integrations/claude-plugin
```

### Claude (claude.ai, Desktop, mobile, Cowork)

Once listed in Anthropic's directory, add **MinutesMaster** from the directory. Until then, add the connector
by hand: **Customize > Connectors > Add custom connector**, name `MinutesMaster`, URL
`https://api.minutesmaster.ai/mcp`, then **Connect**.

### Without OAuth (scripts, CI)

This plugin always signs in with OAuth. For headless use, MinutesMaster also offers personal API keys; the
setup guide at https://www.minutesmaster.ai/ai-agents shows how to add the server with one instead of
this plugin.

## Example prompts

- "What did we decide about the Q1 budget in Monday's leadership meeting? Cite the moments."
- "List my open action items from the last two weeks, grouped by owner."
- "Draft the follow-up email for yesterday's roadmap planning meeting."
- "Prepare me for my call with Kestrel Freight: history, open commitments, questions to ask."
- "Send the MinutesMaster bot to this Teams meeting: <link>" (asks before spending credits)

## Support

- Setup guide and tool list: https://www.minutesmaster.ai/ai-agents
- Support: info@minutesmaster.ai (https://www.minutesmaster.ai/support)
- Disconnect or revoke access any time: https://www.minutesmaster.ai/dashboard/ai-agents

## License

MIT, see [LICENSE](LICENSE). The plugin files are open; the MinutesMaster service has its own
[terms](https://www.minutesmaster.ai/terms).
