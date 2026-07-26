---
name: List recent meetings and pull summaries
description: Page through recent Fireflies meetings and read each meeting's AI summary and action items.
api: https://api.fireflies.ai/graphql
operations: [transcripts, transcript]
---

# List recent meetings and pull summaries

Use the Fireflies GraphQL API at `https://api.fireflies.ai/graphql` with
`Authorization: Bearer <API_KEY>`.

## Steps
1. **List meetings.** Call the `transcripts` query with `skip` and `limit`
   arguments for offset pagination. Filter as documented (e.g. by date or host)
   and select only the fields you need (id, title, date, duration).
2. **Read a summary.** For a chosen id, call the `transcript` query and select the
   `summary` fields (overview, action_items, keywords) plus `sentences` if you need
   the full text.
3. **Iterate.** Increase `skip` by `limit` to page forward; `skip: 0` is valid.

## Rules
- Only request the GraphQL fields you need — this is the point of the GraphQL API.
- Handle `object_not_found` (bad id) and `forbidden` (no access) from `errors[]`.
- Stay within your plan's rate limit; back off on `too_many_requests`.
