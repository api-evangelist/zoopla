# Zoopla (zoopla)

Zoopla Limited is one of the two dominant residential property portals in the United Kingdom, operating zoopla.co.uk alongside PrimeLocation under the Houseful group (formerly ZPG), which also owns the Alto/Jupix estate-agency CRM software and the Hometrack valuation and mortgage-risk business. In a market with no MLS and no cooperative listing standard, Zoopla sits at the demand end of the value chain — consumers search on the portal, and listings reach it from agency CRM systems rather than from a shared data pool. Its API posture reflects that position honestly: the old public Zoopla listings API on the Mashery-hosted developer.zoopla.co.uk portal has been retired and the site no longer serves a valid certificate, and the current developer documentation at developers.zoopla.co.uk states plainly that "the Zoopla listings API is no longer publicly available." What remains public is a small, real, machine-readable surface aimed at member estate agents and their CRM vendors — a Leads API for polling applicant and appraisal enquiries, a real-time lead Push service, and two product-activation APIs for Premium Listings and Weekly Featured Properties. The documentation and three Swagger/OpenAPI contracts are openly readable with no login, but credentials are not self-serve: you must already be a Zoopla member on a listings package, and client_id/client_secret are issued by Member Services after you send them a GPG public key. There is no RESO Web API or Data Dictionary certification, no OData `$metadata` document, and no Universal Property Identifier anywhere in Zoopla's stack — RESO is a North American construct and the UK has not adopted it. Zoopla publishes no open data; the open property layer in the UK belongs to the public sector (HM Land Registry Price Paid and Ordnance Survey), not to the portals.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/zoopla/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/zoopla/refs/heads/main/apis.yml)

## Tags

- Real Estate
- United Kingdom
- Property Listings
- Property Portal
- PropTech
- Rentals
- Estate Agents
- Leads
- CRM Integration

## Timestamps

- **Created:** 2026-07-26
- **Modified:** 2026-07-26

## APIs

### Zoopla Leads API

Poll-based REST API used by Zoopla member agents to retrieve consumer leads generated on the portal. Applicant leads (buyers and renters enquiring on a property) and appraisal leads (owners asking for a valuation) are served from two separate endpoints with separate OAuth2 scopes. Lead data is retained for 30 days. A real-time Push alternative delivers the same payloads to an endpoint you host.

- **Human URL:** [https://developers.zoopla.co.uk/leads/reference/api](https://developers.zoopla.co.uk/leads/reference/api)
- **Base URL:** `https://services.zoopla.co.uk`

#### Tags

- Leads
- Real Estate
- United Kingdom

#### Properties

- [OpenAPI](openapi/zoopla-leads-api-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://developers.zoopla.co.uk/leads/reference/api)
- [Documentation](https://developers.zoopla.co.uk/leads/docs/available-lead-services)
- [Documentation](https://developers.zoopla.co.uk/leads/docs/field-definitions)
- [Webhooks](https://developers.zoopla.co.uk/leads/docs/push-service)
- [Authentication](https://developers.zoopla.co.uk/pages/authentication)
- [Terms of Service](https://developers.zoopla.co.uk/pages/terms-of-use)
- [Support](https://support.zoopla.co.uk/hc/en-gb)

### Zoopla Premium Listing Activations API

REST API that lets a member agent's CRM activate the Premium Listing product against a Zoopla listing, read the activation history for an account or a single listing_id, check whether a listing is premium at the moment of the call, and patch the highlights chart on an activation.

- **Human URL:** [https://developers.zoopla.co.uk/premium-listings/reference/api](https://developers.zoopla.co.uk/premium-listings/reference/api)
- **Base URL:** `https://services.zoopla.co.uk`

#### Tags

- Property Listings
- Real Estate
- United Kingdom

#### Properties

- [OpenAPI](openapi/zoopla-premium-listing-activations-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://developers.zoopla.co.uk/premium-listings/reference/api)
- [Documentation](https://developers.zoopla.co.uk/pages/premium-listings)
- [Documentation](https://developers.zoopla.co.uk/pages/premium-listings-highlights-chart)
- [Errors](https://developers.zoopla.co.uk/pages/premium-listings-error-codes-register)
- [Authentication](https://developers.zoopla.co.uk/pages/authentication)
- [Terms of Service](https://developers.zoopla.co.uk/pages/terms-of-use)

### Zoopla Weekly Featured Property (WFP) Activations API

REST API for activating the Weekly Featured Property product on a Zoopla listing from an agency CRM, reading activation history across an account or for a single listingId, and checking whether a listing is currently configured as a WFP.

- **Human URL:** [https://developers.zoopla.co.uk/wfp/reference/api](https://developers.zoopla.co.uk/wfp/reference/api)
- **Base URL:** `https://services.zoopla.co.uk`

#### Tags

- Property Listings
- Real Estate
- United Kingdom

#### Properties

- [OpenAPI](openapi/zoopla-weekly-featured-property-activations-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://developers.zoopla.co.uk/wfp/reference/api)
- [Documentation](https://developers.zoopla.co.uk/pages/weekly-featured-properties)
- [Errors](https://developers.zoopla.co.uk/pages/weekly-featured-properties-error-code-register)
- [Authentication](https://developers.zoopla.co.uk/pages/authentication)
- [Terms of Service](https://developers.zoopla.co.uk/pages/terms-of-use)

## Common Properties

- [Website](https://www.zoopla.co.uk/)
- [Documentation](https://developers.zoopla.co.uk/)
- [Authentication](https://developers.zoopla.co.uk/pages/authentication)
- [Terms of Service](https://developers.zoopla.co.uk/pages/terms-of-use)
- [Sign Up](https://business.zoopla.co.uk/contact-us)
- [Support](https://support.zoopla.co.uk/hc/en-gb)
- [Business](https://business.zoopla.co.uk/)
- [Parent Company](https://www.houseful.co.uk/)
- [GitHub Organization](https://github.com/zoopla-eng)
- [LinkedIn](https://www.linkedin.com/company/zoopla)

## Access

- **Access gate:** membership-required — you must already be a Zoopla customer subscribed to a listings package with the relevant features.
- **Credentials:** not self-serve. You send Zoopla a GPG public key; Zoopla returns a plaintext `client_id` and a PGP-encrypted `client_secret`. Contact is members@zoopla.co.uk.
- **Auth model:** OAuth 2.0 client credentials against `https://services-auth.services.zoopla.co.uk/oauth2/token` (Amazon Cognito, eu-west-1), HTTP Basic client authentication, Bearer tokens with a 3600-second lifetime.
- **Scopes:** `api/api_access` for the activation APIs; `leads/list:applicant-leads` and `leads/list:appraisal-leads` for the Leads API.
- **RESO posture:** no RESO reference found. No Web API or Data Dictionary certification, no OData `$metadata`, no Universal Property Identifier. The UK has no MLS and no RESO adoption.
- **Open data:** none published by Zoopla. The open UK property layer is HM Land Registry and Ordnance Survey, not the portals.
- **Retired surface:** the public listings API on developer.zoopla.co.uk (legacy TIBCO Mashery) is gone; the host no longer presents a valid certificate. Commercial data conversations go through https://business.zoopla.co.uk/contact-us.

See [review.yml](review.yml) for the full probe log, RESO posture, access gate, and harvest provenance.
