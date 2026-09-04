# Suarify ChatGPT Plugin

Packaging repo for the **Suarify** listing in OpenAI’s universal [Plugins Directory](https://developers.openai.com/plugins/deploy/submission) (ChatGPT + Codex).

Runtime MCP server (not in this repo):

- URL: `https://mcp.suarify.my/mcp`
- Source: `suarify-avr` → `suarify-connect-mcp`
- Auth: OAuth 2.1 (Google / email OTP)

## Layout

| Path | Purpose |
|------|---------|
| `.codex-plugin/plugin.json` | Plugin identity + listing metadata |
| `.mcp.json` | Points ChatGPT/Codex at the hosted MCP |
| `skills/suarify-voice-calls/` | Workflow skill for voice calling |
| `assets/logo.png` | Directory logo |
| `SUBMISSION.md` | Portal form copy, prompts, test cases |
| `.agents/plugins/marketplace.json` | Local marketplace for desktop testing |

## Local test (Developer Mode)

1. ChatGPT → Settings → Security → enable **Developer mode**
2. Add connector / MCP: `https://mcp.suarify.my/mcp` → complete Google/OTP
3. Optional: install this folder via a personal marketplace (see marketplace.json)
4. Confirm tools work (profile, listAgents, getTopUpLink)

## Public publish

Follow **SUBMISSION.md**. High level:

1. Verify business identity + Apps Management write in OpenAI Platform
2. Plugin submission portal → **With MCP** → Universal URL `https://mcp.suarify.my/mcp`
3. Domain verify `mcp.suarify.my` via `/.well-known/openai-apps-challenge`
4. **Scan Tools**, fix annotations if needed, submit review materials
5. After approval → **Publish** from the portal

Docs:

- [Submit plugins](https://developers.openai.com/plugins/deploy/submission)
- [Package your plugin](https://developers.openai.com/plugins/build/plugins)
- [MCP review requirements](https://developers.openai.com/plugins/deploy/app-review)
