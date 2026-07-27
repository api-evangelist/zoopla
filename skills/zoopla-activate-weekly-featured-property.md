---
name: Activate a Zoopla Weekly Featured Property from a CRM
description: >-
  Spend a Weekly Featured Property credit against a UK residential listing, poll
  the asynchronous activation, carry your own order reference through
  customDetails, and read the 201xxxx error register.
api: openapi/zoopla-weekly-featured-property-activations-openapi.json
operations:
  - POST /products/weekly-featured-properties
  - GET /products/weekly-featured-properties/{uuid}
  - GET /products/weekly-featured-properties
scopes:
  - api/api_access
---

# Activate a Weekly Featured Property

Same shape as Premium Listings, different eligibility rules and a different error
range. **It spends a credit on the live contract — there is no sandbox.**

## 1. Token

```
POST https://services-auth.services.zoopla.co.uk/oauth2/token
Content-Type: application/x-www-form-urlencoded
Authorization: Basic base64(client_id:client_secret)

grant_type=client_credentials&scope=api/api_access
```

Cache it; send `Authorization: Bearer {access_token}`.

## 2. Eligibility

A WFP activation is rejected unless the listing:

- exists and belongs to you (`2011001`-`2011002`, permission errors in the 900 range)
- has **at least 1 image** (`2011007`)
- is a **residential** property in a valid category (`2011009`)
- is **located in the UK** (`2011008`)
- has no pending (`2011004`) or already-activated (`2011005`) WFP

Check the last one first:

```
GET /products/weekly-featured-properties?listingId=123&isWFP=true
```

`[]` means it is not currently featured.

## 3. Request the activation

```
POST /products/weekly-featured-properties
Content-Type: application/json

{ "listingId": 123 }
```

Carry your own reference through if you want it echoed back on the resource:

```json
{ "listingId": 123, "customDetails": { "orderNumber": 456 } }
```

You get **202 Accepted** with a `PENDING` activation: uuid `id`, `createdBy`
(your client_id), `isRenewable`, and your `customDetails` echoed.

## 4. Poll

```
GET /products/weekly-featured-properties/{uuid}
```

until `status.result` is `ACTIVATED` (with `expiryAt`) or `ERROR` (with
`status.errors[]`).

## 5. No idempotency key

Same guard as Premium Listings. A repeated POST for a listing already in flight
or already featured is rejected with `2011004` / `2011005` — treat those as
"already in progress" and "already done", not as failures. Always look up by
`listingId` before retrying a POST whose response you lost.

## 6. Errors

Envelope: `{"errors": [{"reason": "…", "code": 2011004}]}`. Codes are seven
digits with `201` identifying the WFP API. The full register is in
`errors/zoopla-error-codes.yml`.

## 7. Listing history

`GET /products/weekly-featured-properties` returns a bare array with **no
pagination**; narrow it with `listingId` and `isWFP`, combined with AND.

## 8. Timing

Do not attempt an activation within 30 minutes of submitting a new listing —
Zoopla needs to assign it an identifier first.
