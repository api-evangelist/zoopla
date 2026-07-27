---
name: Receive Zoopla leads in real time over the Push Service
description: >-
  Stand up the endpoint Zoopla POSTs leads to, authenticate Zoopla as the
  incoming client, distinguish applicant from appraisal payloads, and survive the
  24-hour retry window.
api: asyncapi/zoopla-leads-push-asyncapi.yml
operations:
  - receiveApplicantLead
  - receiveAppraisalLead
docs: https://developers.zoopla.co.uk/leads/docs/push-service
---

# Receive Zoopla lead pushes

The Push Service inverts the usual relationship: **Zoopla is the client and you
are the server.** You host an HTTPS endpoint, Zoopla POSTs one lead per request
in real time, and Zoopla authenticates itself to you using credentials you
supply.

## 1. Decide your endpoint layout

Either one endpoint for everything, or separate endpoints for applicant and
appraisal leads. If you share one endpoint, tell the two apart by the **top-level
key** on the body: `"applicant"` or `"appraisal"`.

## 2. Choose how Zoopla authenticates to you

Zoopla supports exactly two inbound methods.

**OAuth 2.0 (client credentials only).** Give Zoopla a `client_id`,
`client_secret`, token endpoint and — depending on configuration — an audience.
Zoopla mints a token and sends `Authorization: Bearer {access_token}`.

**API key.** Give Zoopla a key. Zoopla sends it verbatim:
`Authorization: {api-key}` — with no `Bearer` prefix. Validate the header value
as an exact match.

There is **no request signature, no HMAC and no timestamp header**, so the
Authorization header is the whole of your authenticity check. Terminate TLS
properly and rate-limit the endpoint.

## 3. Set your filters

Four independent booleans, configured with Zoopla per endpoint: applicant leads,
appraisal leads, sales leads, lettings leads. Ask for only what you will process.

## 4. Parse the payload

```json
{ "applicant": { "id": "…", "contact": {…}, "listingDetails": {…}, … } }
{ "appraisal": { "id": "…", "contact": {…}, "propertyDetails": {…}, … } }
```

Full published examples: `examples/zoopla-push-applicant-lead.json` and
`examples/zoopla-push-appraisal-lead.json`. Schemas:
`openapi/zoopla-leads-api-openapi.json#/definitions/Applicant` and
`#/definitions/Appraisal`.

## 5. Answer fast, and be idempotent yourself

- Acknowledge with a 2xx as soon as you have durably stored the lead; do your
  CRM work asynchronously.
- Zoopla retries a failing endpoint **for 24 hours**. Retries can therefore
  re-deliver a lead you already have — **de-duplicate on the lead `id`**, which
  is a uuid.
- After 24 hours there is no automatic replay. Leads are held for the remaining
  29 days (30 total) and recovery is a manual conversation with Zoopla.
- Backstop: the same leads are pollable from the Leads API for 30 days. If your
  endpoint was down longer than the retry window, backfill with
  `skills/zoopla-poll-leads.md`.

## 6. Treat the contract as additive

"New fields and enum values can be added at any time." Never reject a payload for
an unknown field or enum member. Sanitise the consumer-entered `message` field
before display.
