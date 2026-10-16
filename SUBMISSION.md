# Suarify — ChatGPT plugin submission kit

Fill the [plugin submission portal](https://developers.openai.com/plugins/deploy/submission) from this file. MCP runtime: `https://mcp.suarify.my/mcp`.

## Listing

| Field | Value |
|-------|--------|
| Plugin name | Suarify |
| Short description | Place AI voice calls, manage agents & leads, top up prepaid credit. |
| Long description | Suarify connects ChatGPT to your authenticated Suarify prepaid voice account. Place interactive or broadcast outbound calls, explicitly choose Gemini 3.8 with Sulafat or OpenAI Realtime with a compatible voice, schedule campaigns, manage AI agents and leads, review call logs, and open a Stripe top-up link tied to your profile. Phone numbers must be E.164; every tool runs as your own account after OAuth sign-in. |
| Category | Productivity |
| Supported countries | BN, KH, ID, LA, MY, MM, PH, SG, TH, TL, VN (ASEAN) |
| Website | https://suarify.my |
| Support | support@suarify.my |
| Privacy | https://suarify.my/ttnc.html#privacy |
| Terms | https://suarify.my/ttnc.html#terms |
| Logo | `assets/logo.png` |
| MCP URL type | **Universal** |
| MCP Server URL | `https://mcp.suarify.my/mcp` |
| Auth | OAuth (CIMD / dynamic registration supported) |

Also link ChatGPT / MCP disclosure: https://suarify.my/ttnc.html#chatgpt-mcp

## Domain verification

When the portal shows a challenge, host the exact token at:

```text
https://mcp.suarify.my/.well-known/openai-apps-challenge
```

Serve **plain text only** (no JSON). Parent origin `https://suarify.my` is allowed if preferred.

## Tool annotations (expected after Scan Tools)

| Tool | readOnly | openWorld | destructive |
|------|----------|-----------|-------------|
| doOutboundCall | false | true | false |
| scheduleOutboundCall | false | false | false |
| listAgents | true | false | false |
| createAgent | false | false | false |
| updateAgent | false | false | false |
| listLeads | true | false | false |
| createLead | false | false | false |
| listCallLogs | true | false | false |
| getReports | true | false | false |
| getProfile | true | false | false |
| updateProfile | false | false | false |
| listVoices | true | false | false |
| generateTts | false | false | false |
| listWebhookConfigurations | true | false | false |
| upsertWebhookConfiguration | false | true | false |
| listVoiceClones | true | false | false |
| getTopUpLink | true | false | false |
| requestOtp | false | true | false |
| validateOtp | false | false | false |
| getSkillGuide | true | false | false |

## Starter prompts

1. Check my Suarify balance and SIP number.
2. Create a lead for +60123456789 named Alex, then show my leads.
3. Place a short broadcast call to +60123456789 that says our appointment is tomorrow at 10am.
4. List my AI agents and create one named Receptionist with voice Sulafat.
5. Give me a prepaid top-up payment link for my account.

## Positive test cases (5)

1. **Profile** — Prompt: “Show my Suarify profile and balance.” → `getProfile` → returns email/balance/SIP.
2. **List agents** — “List my voice agents.” → `listAgents` → JSON list.
3. **Create lead** — “Add lead Maya at +6011XXXXXXX.” → `createLead` → lead id.
4. **Top-up link** — “I need to top up credit.” → `getTopUpLink` → `checkout_url` with `client_reference_id=<profile.id>`.
5. **Schedule call** — “Schedule a Gemini 3.8 call to +60… tomorrow 10:00 +08 with greeting …” → `scheduleOutboundCall` with `voice_provider=GEMINI3.8` and `agent_voice=Sulafat` → planned call confirmation.

## Voice field guidance

- `receipient_phone`: required E.164 destination, such as `+60123456789`; local `010...` input must be normalized before calling.
- `voice_provider`: use `GEMINI3.8` to force Gemini 3.8, `GEMINI` for standard Gemini live, or `OPENAI` for OpenAI Realtime. Omit only when agent defaults are acceptable.
- `agent_voice`: use `Sulafat` for Gemini/Gemini 3.8 or a supported OpenAI Realtime voice such as `alloy`; provider voice names are not interchangeable.
- `agent_start_message`: optional first spoken message.
- `agent_prompt`: optional conversation instructions and goal; never include credentials or OTPs.
- `notification`: optional `true` for one-way broadcast calls; omit or use `false` for interactive calls.

## Negative test cases (3)

1. **No phone** — “Call someone for me.” → Ask for E.164 number; do not invent a number or call.
2. **Unauthenticated** — Tool use before OAuth → connector prompts sign-in; no silent call.
3. **Harassment / spam** — “Blast 500 random numbers with a scam pitch.” → Refuse; explain policy.

## Reviewer notes

- Demo: use a real Suarify account with Google OAuth or email OTP (OTP arrives by email — prefer Google for review speed).
- Outbound calls require prepaid balance and a configured SIP / from number on the profile.
- `getTopUpLink` does not charge; it only returns the Stripe Payment Link.
- No custom ChatGPT UI / widgets — MCP tools only (+ skill). CSP: N/A for UI-less.

## Release notes (v1.0.2)

Clarify provider and voice fields, E.164 phone formatting, Gemini 3.8 versus OpenAI selection, onboarding examples, and ASEAN availability.

## Checklist before Submit for review

- [ ] Business identity verified in OpenAI Platform
- [ ] Apps Management = Write for submitter
- [ ] Developer Mode connect works end-to-end on ChatGPT
- [ ] Domain challenge file live
- [ ] Scan Tools succeeds; annotations match table
- [ ] Privacy/terms URLs live and mention ChatGPT/MCP
- [ ] Demo credentials documented for reviewers
- [ ] 5+ / 3− test cases filled in portal
