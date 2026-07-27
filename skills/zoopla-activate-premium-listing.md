---
name: Activate a Zoopla Premium Listing from a CRM
description: >-
  Spend a Premium Listing credit against a Zoopla listing, poll the asynchronous
  activation to completion, set or update the highlights chart, and read the
  numeric error register correctly — including the duplicate guard that stands in
  for idempotency.
api: openapi/zoopla-premium-listing-activations-openapi.json
operations:
  - POST /products/premium-listings
  - GET /products/premium-listings/{uuid}
  - GET /products/premium-listings
  - PATCH /products/premium-listings/{uuid}
scopes:
  - api/api_access
---

# Activate a Premium Listing

**This spends money on the first call.** Zoopla publishes no sandbox and no test
mode; `services.zoopla.co.uk` is the production host and an activation consumes a
credit on the member's live contract. Gate this behind an explicit human decision.

## 1. Get a token

```
POST https://services-auth.services.zoopla.co.uk/oauth2/token
Content-Type: application/x-www-form-urlencoded
Authorization: Basic base64(client_id:client_secret)

grant_type=client_credentials&scope=api/api_access
```

Cache the `access_token` for its `expires_in` (3600s) and send it as
`Authorization: Bearer {access_token}`.

## 2. Check eligibility before you spend

Zoopla enforces quality gates server-side, but checking first avoids a wasted
round trip and a confusing error:

- Full description of at least **200 characters** (else `1011007`)
- At least **3 images** on the listing (else `1011008`)
- Custom details under **640kb** (else `1011009`)
- The listing must already have a Zoopla identifier. **Wait at least 30 minutes
  after submitting a new listing** before attempting to upgrade it.

Then confirm it is not already premium:

```
GET /products/premium-listings?listingId=123&isPremium=true
```

An empty array `[]` means not premium. A non-empty array means it already is —
stop here.

## 3. Request the activation

```
POST /products/premium-listings
Content-Type: application/json

{ "listingId": 123 }
```

Optionally include highlights, whose ids come from the published highlights
chart:

```json
{ "listingId": 123, "highlights": [ { "id": 456, "description": "optional text" } ] }
```

You get **202 Accepted** with a `PENDING` activation carrying a uuid `id`.
Persist that uuid — it is your only handle on the request.

## 4. Poll it to completion

```
GET /products/premium-listings/{uuid}
```

`status.result` moves from `PENDING` to either:

- `ACTIVATED` — the product is live, and `expiryAt` tells you when it lapses
- `ERROR` — `status.errors[]` carries `{reason, code}` from the register

Poll with backoff. This is a queue, not a synchronous call.

## 5. There is no idempotency key — read the duplicate codes as success

Zoopla accepts no `Idempotency-Key`. If a retry lands after the first request was
already accepted, you get a rejection, not a replay:

| Code | Meaning | How to treat it |
|---|---|---|
| 1011003 | A pending activation already exists for this listingId | Already in flight — go find it and poll, do not retry |
| 1011004 | An activated premium listing already exists for this listingId | Already done — succeed |

Before retrying a POST whose response you never saw, always
`GET /products/premium-listings?listingId={id}` first.

## 6. Update the highlights chart

```
PATCH /products/premium-listings/{uuid}
{ "highlights": [ { "id": 42 } ] }
```

Only an activation that is `ACTIVATED` **and** not yet expired can be patched
(`1011043`), and only one update may be in flight at a time (`1011044`). The
response shows `status.currently: WAITING_FOR_UPDATE`; re-GET after about a
second for the settled representation.

## 7. Errors

The envelope is `{"errors": [{"reason": "…", "code": 1011001}]}`. Codes are
seven digits: service (101 API / 102 processor), severity, feature code. The full
register is in `errors/zoopla-error-codes.yml`. Notable ones:

- `1011901` — client does not have access to that listing
- `1021006` — the member contract has no premium features
- `1011051` / `1011061` — listing or customerListingId could not be resolved
- `1011065` — `listingId` and `customerListingId` cannot both be set

## 8. Listing history

`GET /products/premium-listings` returns everything the caller can see, as a bare
array. **There is no pagination.** Narrow with `listingId` and `isPremium`, which
combine with logical AND.
