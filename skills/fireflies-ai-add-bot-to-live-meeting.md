---
name: Add the Fireflies bot to a live meeting
description: Send the Fireflies (Fred) notetaker bot into an ongoing meeting and track it as an active meeting.
api: https://api.fireflies.ai/graphql
operations: [addToLive, activeMeetings]
---

# Add the Fireflies bot to a live meeting

Use the Fireflies GraphQL API at `https://api.fireflies.ai/graphql` with
`Authorization: Bearer <API_KEY>`.

## Steps
1. **Add the bot.** Call the `addToLive` mutation with the meeting-link URL of a
   supported platform. Unsupported links return `unsupported_platform`.
2. **Confirm it joined.** Optionally listen for the `meeting.bot_joined` webhook,
   or call the `activeMeetings` query to see in-progress/paused meetings.
3. **When finished**, the meeting flows through the normal pipeline — a
   `meeting.transcribed` webhook fires and you can fetch it with the `transcript`
   query.

## Rules
- `addToLive` is rate limited to 3 requests per 20 minutes — do not retry tightly.
- Handle `paid_required` (feature needs a paid tier) and `forbidden` from the
  GraphQL `errors[]` envelope.
