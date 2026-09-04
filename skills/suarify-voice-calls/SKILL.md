---
name: suarify-voice-calls
description: Place and schedule Suarify AI voice calls, manage agents/leads, check balance, and top up prepaid credit via MCP tools.
---

# Suarify voice calling

Use the **suarify** MCP tools after the user has completed OAuth (Google or email OTP).

## When to use
- User wants to call a phone number with an AI agent
- Broadcast / one-way notification calls
- Schedule a future outbound call
- Check balance, profile, call logs, or get a top-up payment link
- Create or update agents and leads

## Defaults
- `voice_provider`: GEMINI
- `agent_voice`: Sulafat
- Phone numbers must be E.164 (e.g. `+60123456789`)

## Call flow
1. Confirm recipient phone and whether the call is interactive or `notification: true` (broadcast).
2. Confirm the opening line (`agent_start_message`) and instructions (`agent_prompt`) for interactive calls.
3. Call `doOutboundCall` (immediate) or `scheduleOutboundCall` (planned).
4. Share call/token ids from the response; use `listCallLogs` to follow up.

## Billing
- Use `getProfile` for balance.
- Use `getTopUpLink` and show the user `checkout_url` (Stripe Payment Link with their profile id). Do not invent payment URLs.

## Safety
- Do not place calls without a clear recipient and purpose from the user.
- Do not expose API keys or OTP codes in chat.
- Refuse requests that look like spam/harassment or illegal robocalling.
