---
name: Upload audio and fetch its transcript
description: Upload a recording to Fireflies and retrieve the finished transcript, summary, and action items.
api: https://api.fireflies.ai/graphql
operations: [uploadAudio, transcript]
---

# Upload audio and fetch its transcript

Use the Fireflies GraphQL API at `https://api.fireflies.ai/graphql`.

## Auth
Send `Authorization: Bearer <API_KEY>` on every request. Generate the key in the
Fireflies app under Settings > Developer Settings.

## Steps
1. **Upload the audio.** Call the `uploadAudio` mutation with the hosted audio
   `url`, a `title`, and (optionally) a `client_reference_id` you generate so you
   can correlate the later webhook. Audio uploads are limited to 200MB and must be
   at least 50KB (`payload_too_small` otherwise). Transcription is asynchronous.
2. **Wait for completion.** Either subscribe to the `meeting.transcribed` webhook
   (Webhooks V2, verified via the `X-Hub-Signature` HMAC-SHA256 header) and read
   `meeting_id` + `client_reference_id`, or poll.
3. **Fetch the transcript.** Call the `transcript` query with the returned id to
   read sentences, speakers, timestamps, and the summary (overview, action items,
   keywords).

## Rules
- Respect rate limits: 50/day (Free), 500/day (Pro), 60/min (Business/Enterprise).
  On `too_many_requests` back off until the `extensions.metadata.retryAfter` time.
- Errors arrive in the GraphQL `errors[]` array with `extensions.code` /
  `extensions.status` — see errors/fireflies-ai-error-codes.yml.
