---
name: suarify-voice-calls
description: Place and schedule authenticated Suarify AI voice calls, choose the Gemini 3.8 or OpenAI voice provider explicitly, manage agents and leads, inspect call logs, check balance, and top up prepaid credit via MCP tools.
---

# Suarify voice calling

Use the **suarify** MCP tools after the user has completed OAuth (Google or email OTP).

## When to use
- User wants to call a phone number with an AI agent
- Broadcast / one-way notification calls
- Schedule a future outbound call
- Check balance, profile, call logs, or get a top-up payment link
- Create or update agents and leads

## Call fields and defaults
- `receipient_phone` (required): the destination number in E.164 format, for example `+60123456789`. Do not use a local `010...` number without converting it to `+601...`.
- `voice_provider` (optional): the speech-to-speech backend. Use `GEMINI3.8` to force Gemini 3.8, `GEMINI` for the standard Gemini live route, or `OPENAI` for OpenAI Realtime. If omitted, the configured agent may select the provider.
- `agent_voice` (optional): the provider-compatible voice name. Use `Sulafat` with Gemini/Gemini 3.8; use a supported OpenAI Realtime voice such as `alloy` with `OPENAI`. Do not send an ElevenLabs voice id to Gemini or OpenAI.
- `agent_start_message` (optional): the first sentence spoken when the recipient answers.
- `agent_prompt` (optional): the agent's instructions and conversation goal. Keep it specific and do not put secrets or OTPs in it.
- `notification` (optional): set `true` for a one-way broadcast message; otherwise the call is interactive.
- Phone numbers must be E.164 (e.g. `+60123456789`).

When the user asks for Gemini 3.8, always include both `voice_provider: "GEMINI3.8"` and a compatible voice such as `agent_voice: "Sulafat"`; do not rely on the agent's default. After changing provider or voice, report the selected values in the confirmation.

## Call flow
1. Confirm recipient phone and normalize it to E.164. Explain the selected provider and voice before calling.
2. Confirm whether the call is interactive or `notification: true` (broadcast).
3. Confirm the opening line (`agent_start_message`) and instructions (`agent_prompt`) for interactive calls.
4. Call `doOutboundCall` (immediate) or `scheduleOutboundCall` (planned), passing the provider and voice fields when the user selected them.
5. Share call/token ids from the response; use `listCallLogs` to follow up.

## Billing
- Use `getProfile` for balance.
- Use `getTopUpLink` and show the user `checkout_url` (Stripe Payment Link with their profile id). Do not invent payment URLs.

## Safety
- Do not place calls without a clear recipient and purpose from the user.
- Do not expose API keys or OTP codes in chat.
- Refuse requests that look like spam/harassment or illegal robocalling.
