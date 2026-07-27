---
name: Poll Zoopla for new applicant and appraisal leads
description: >-
  Authenticate as a Zoopla member agency and pull new consumer leads — buyers and
  renters enquiring on a property (applicant), and owners asking for a valuation
  (appraisal) — on a schedule, without losing or double-processing leads.
api: openapi/zoopla-leads-api-openapi.json
operations:
  - GET /applicant-leads
  - GET /appraisal-leads
scopes:
  - leads/list:applicant-leads
  - leads/list:appraisal-leads
---

# Poll Zoopla for leads

Zoopla's Leads API is a poll surface: you ask for a time window and get every
lead in it. There is no cursor, no page token and no delivery receipt — the time
window is the only state, so you have to keep it yourself.

## Before you start

- You must already be a Zoopla member on a listings package with the leads
  feature. There is no self-serve signup and no sandbox.
- `client_id` and `client_secret` are issued by Zoopla Member Services after you
  send them a 4096-bit RSA GPG public key; the secret comes back PGP-encrypted.
- Base host: `https://services.zoopla.co.uk`

## 1. Get a token

```
POST https://services-auth.services.zoopla.co.uk/oauth2/token
Content-Type: application/x-www-form-urlencoded
Authorization: Basic base64(client_id:client_secret)

grant_type=client_credentials
```

The response carries `access_token`, `token_type: Bearer` and `expires_in: 3600`.

**Cache the token for its full lifetime.** Minting a token per request is the
documented cause of `429` responses on this API.

## 2. Pull applicant leads

`GET /applicant-leads` with `Authorization: Bearer {access_token}`.

With no parameters you get the last 24 hours. Narrow it with:

| Parameter | Meaning |
|---|---|
| `from-time` | Only leads on or after this time (`2006-01-02T15:04:05Z`) |
| `to-time` | Only leads on or before this time |
| `group-id` | Restrict to a Zoopla group |
| `company-id` | Restrict to a Zoopla company |
| `brand-id` | Restrict to a Zoopla brand |
| `branch-id` | Restrict to a Zoopla branch |

Response is `{"applicants": [ … ]}`. Each applicant carries `id` (uuid),
`contact`, `intent`, `enquiryType`, `message`, `searchCriteria` and
`listingDetails` for the property enquired about. See
`examples/zoopla-push-applicant-lead.json` for the published shape.

## 3. Pull appraisal leads

`GET /appraisal-leads`, same six parameters, same auth, different scope. Response
is `{"appraisals": [ … ]}`; each carries `propertyDetails` with an automated
`estimate`, `tenure`, `energy` rating and `uprn`.

## 4. Keep your own watermark

- Store the `to-time` you last used and start the next poll from it. Overlap the
  window by a minute rather than leaving a gap; de-duplicate on the lead `id`.
- Leads are retained for **30 days**, so a poller that has been down can backfill
  by widening `from-time` — but only within that window.
- Both endpoints are safe to repeat: they are `GET`s with no side effects.

## 5. Handle the failures

| Status | What it means | What to do |
|---|---|---|
| 400 | Bad request | Fix the parameters; check the timestamp format |
| 401 | Token invalid or expired | Mint a new token and retry once |
| 403 | Forbidden | Your credentials lack the lead scope — contact Member Services |
| 429 | Service busy | Wait and retry with backoff; verify you are caching tokens |
| 500 | Internal server error | Retry with backoff |

## 6. Treat the payload as open

Zoopla warns that new fields and new enum values can appear at any time. Ignore
unknown fields; never fail on an unrecognised enum member. **Sanitise the
`message` field before you display it** — it is consumer-entered free text and
may contain HTML.

## When to use push instead

If latency matters, ask Zoopla to configure the Lead Push Service instead: the
same payloads are POSTed to an endpoint you host in real time. See
`skills/zoopla-receive-lead-push.md`.
