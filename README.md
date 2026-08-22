# Zoopla (zoopla)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
