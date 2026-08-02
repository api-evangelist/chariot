---
name: Subscribe to Chariot webhooks
description: Create an event subscription and consume Chariot events for grants, donations, disbursements, and transfers.
api: openapi/chariot-openapi-original.yml
operations: [createEventSubscription, listEventSubscriptions, getEventSubscription, updateEventSubscription, listEvents, getEvent]
---

# Subscribe to Chariot webhooks

Use this flow to receive Chariot events instead of polling resources.

## Auth & environment
- HTTP bearer secret key (`Authorization: Bearer {TOKEN}`; test prefix `sk_test_`).

## Steps
1. **Create a subscription** — `createEventSubscription` with your endpoint URL and the event categories you care about (e.g. `grant.created`, `grant.updated`, `donation.created`, `disbursement.updated`, `inbound_transfer.updated`).
2. **Manage subscriptions** — `listEventSubscriptions`, `getEventSubscription`, `updateEventSubscription`.
3. **Handle the webhook** — the payload is an `Event` object: `id`, `category`, `created_at`, `associated_object_id`, `associated_object_type`. Status transitions arrive as `{resource}.updated`; fetch the object via its resource API to read current state.
4. **Backfill / reconcile** — pull history with `listEvents` (`limit` + `cursor`) and `getEvent`.

## Conventions
- Event names follow `{resource}.{event_type}` where event_type is `created` or `updated`.
- Errors are RFC 7807 `application/problem+json`; every response carries `X-Request-Id`.
