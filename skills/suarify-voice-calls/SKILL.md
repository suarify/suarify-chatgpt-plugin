---
name: suarify-voice-calls
description: Place, schedule, or start authenticated Suarify voice calls via MCP tools.
---

# Suarify voice calling

Use the **suarify** MCP tools after the user has completed OAuth (Google or email OTP). The public connector supports one-way reminders and immediate two-way calls, but does not expose account administration or call history.

## When to use
- User wants to place an immediate one-way reminder call
- Schedule a future outbound call
- User explicitly wants an immediate two-way call

## Call fields and defaults
- `receipient_phone` (required): the destination number in E.164 format, for example `+60123456789`. Do not use a local `010...` number without converting it to `+601...`.
- `agent_start_message` (optional): the first sentence spoken when the recipient answers.
- `agent_prompt` (optional): the reminder content supplied by the user. Do not put secrets or OTPs in it.
- `agent_prompt` (required for two-way calls): the conversation goal and boundaries supplied by the user. Keep it narrow and do not put secrets or OTPs in it.
- `scheduled_at` (required for scheduling): ISO 8601 time with timezone.
- Phone numbers must be E.164 (e.g. `+60123456789`).

## Call flow
1. Confirm recipient phone and normalize it to E.164.
2. Confirm the reminder message and, for scheduling, the time zone and scheduled time.
3. Call `suarify_initiate_call` for an immediate reminder, `suarify_schedule_call` for a planned reminder, or `suarify_start_two_way_call` for an immediate two-way call.
4. Return only the safe accepted/scheduled status and optional reference from the tool.

## Safety
- Do not place calls without a clear recipient and purpose from the user.
- Do not expose API keys or OTP codes in chat.
- Refuse requests that look like spam/harassment or illegal robocalling.
- Do not request or reveal profile, balance, lead, agent, recording, call-log, or payment data through the public connector.
- For two-way calls, confirm the user supplied the purpose and boundaries; do not invent a conversation goal.
