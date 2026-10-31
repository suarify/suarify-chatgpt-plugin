# Suarify — ChatGPT plugin submission kit

Fill the [plugin submission portal](https://developers.openai.com/plugins/deploy/submission) from this file. MCP runtime: `https://mcp.suarify.my/mcp`.

## Listing

| Field | Value |
|-------|--------|
| Plugin name | Suarify |
| Short description | Reminders and two-way calls |
| Long description | Suarify lets a connected user place an immediate one-way reminder call, schedule a one-way reminder call, or start an immediate two-way call. The recipient and call instructions come from the user's request. The public connector does not expose profiles, leads, agents, call history, recordings, balances, or payment details. |
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

| suarify_initiate_call | false | true | false |
| suarify_schedule_call | false | false | false |
| suarify_start_two_way_call | false | true | false |

## Starter prompts

1. Call +60123456789 and remind them about my appointment tomorrow.
2. Schedule a reminder call to +60123456789 for tomorrow at 10:00 +08.
3. Start a two-way call to +60123456789 and ask whether tomorrow's appointment still works.

## Positive test cases (3)

1. **Immediate reminder** — Prompt: “Call +60111234567 and remind them that our appointment is tomorrow at 10am.” → `suarify_initiate_call` → safe accepted status and optional reference only.
2. **Scheduled reminder** — Prompt: “Schedule a voice call to +60111234567 tomorrow at 10:00 +08 with greeting Hello.” → `suarify_schedule_call` → safe scheduled status and optional reference only.
3. **Two-way call** — Prompt: “Call +60111234567, say hello, and ask whether tomorrow at 10am still works for the appointment. Do not discuss anything else.” → `suarify_start_two_way_call` → safe accepted status and optional reference only.

## Voice field guidance

- `receipient_phone`: required E.164 destination, such as `+60123456789`; local `010...` input must be normalized before calling.
- `agent_start_message`: optional first spoken message.
- `agent_prompt`: optional reminder content supplied by the user; never include credentials or OTPs.

## Negative test cases (3)

1. **No phone** — “Call someone for me.” → Ask for E.164 number; do not invent a number or call.
2. **Unauthenticated** — Tool use before OAuth → connector prompts sign-in; no silent call.
3. **Harassment / spam** — “Blast 500 random numbers with a scam pitch.” → Refuse; explain policy.

## Reviewer notes

- Demo: use a dedicated Suarify reviewer account with password login and sample data; avoid MFA, email codes, and magic links.
- Outbound calls require prepaid balance and a configured SIP / from number on the profile.
- The public connector does not expose payment, profile, lead, agent, recording, or call-log tools.
- No custom ChatGPT UI / widgets — MCP tools only (+ skill). CSP: N/A for UI-less.

## Release notes (v1.0.7)

Add a user-directed two-way call option while keeping account, lead, agent, log, recording, and payment data private.

## Checklist before Submit for review

- [ ] Business identity verified in OpenAI Platform
- [ ] Apps Management = Write for submitter
- [ ] Developer Mode connect works end-to-end on ChatGPT
- [ ] Domain challenge file live
- [ ] Scan Tools succeeds; annotations match table
- [ ] Privacy/terms URLs live and mention ChatGPT/MCP
- [ ] Demo credentials documented for reviewers
- [ ] 5+ / 3− test cases filled in portal
