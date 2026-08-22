# Fluxx (fluxx-grants)

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

Fluxx is a cloud grants management platform for foundations, government agencies, corporations, and nonprofits, covering the full grant lifecycle - from funding announcement through pre-award applications, post-award payments, and measurement and evaluation. Founded in 2010 and headquartered in San Francisco, Fluxx is "purpose-built by grantmakers for grantmaking" and ranks as a leading grants management solution in the Technology Association of Grantmakers survey.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/fluxx-grants/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/fluxx-grants/refs/heads/main/apis.yml)

## Access Model (Read This First)

Fluxx exposes a **real, documented REST API (REST API v2)** - but it is **not a public, self-serve developer API**. Access is **gated per customer instance**:

- The API is included with **Fluxx Grantmaker** (enterprise, contact-sales) and is enabled per customer.
- It is served from each customer's own Fluxx deployment under `https://{client}.fluxx.io/api/rest/v2`.
- The **reference documentation is generated per instance** at `/api/rest/v2/doc` (for example `/api/rest/v2/doc/GrantRequest`), so there is no single hosted public API reference or OpenAPI to link to.
- The data model is **dynamic and instance-specific**: available models, fields, and permissions are configured per deployment.

Because the surface lives behind an authenticated customer instance, the endpoints in this catalog are **honestly modeled** (`endpointsModeled`) from Fluxx's documented REST v2 conventions and from third-party integrations, rather than harvested from a public reference. No OpenAPI, plans, rate-limit, FinOps, or collection artifacts were fabricated.

## Design: Per-Client Instance + Dynamic Model

Fluxx is a multi-tenant SaaS where each customer gets its own instance (subdomain). The REST API mirrors that:

- **Per-client base URL** - every request targets the customer's own host, e.g. `https://acme-foundation.fluxx.io/api/rest/v2`.
- **Dynamic model API** - records are accessed generically as `/api/rest/v2/{model}` (e.g. `grant_request`, `organization`, `user`, `request_transaction`, `model_document`). The set of models and their fields is defined by the instance's configuration, and the per-instance `/api/rest/v2/doc` endpoint enumerates what is actually available for that deployment.
- **OAuth 2.0** - an administrator registers an application at `/oauth/applications` on the instance to get a client ID and secret, then exchanges them for an access token at `/oauth/token`.
- **Query conventions** - list requests support `cols` (select fields/relations), `filter` (three-part `field relator value` criteria such as `grant_id eq R-2024-00003` or `created_at last-n-months 5`), and `page` / `per_page` pagination.

## APIs

### Fluxx Records API

RESTful create, list, get, update, and delete access to Fluxx model records such as `GrantRequest`, `Organization`, and `RequestTransaction` under `/api/rest/v2/{model}`. Supports column selection, filtering, and pagination. Modeled from REST v2 conventions; concrete fields and available models are per instance.

- **Base URL:** `https://{client}.fluxx.io/api/rest/v2`

### Fluxx OAuth API

OAuth 2.0 authorization for the REST API. Register an application at `/oauth/applications`, obtain a client ID and secret, then exchange them for an access token at `/oauth/token`.

- **Base URL:** `https://{client}.fluxx.io/oauth`

### Fluxx Documents API

Access to files and documents attached to grant records via Fluxx document models (such as `ModelDocument`) under `/api/rest/v2/{model}`. Used to list and retrieve attachments associated with a `GrantRequest` or other record.

- **Base URL:** `https://{client}.fluxx.io/api/rest/v2`

### Fluxx Users and Organizations API

RESTful access to Fluxx people and organization records - the `User` and `Organization` models under `/api/rest/v2/{model}` that represent grantees, grantseekers, staff, and funding organizations.

- **Base URL:** `https://{client}.fluxx.io/api/rest/v2`

## Pricing

Fluxx Grantmaker (the grantmaker/foundation product that includes the REST API) is **enterprise, contact-sales** - no public pricing is published; the site directs prospects to request a demo and contact sales. A separate consumer-facing product, Fluxx Grantseeker (for grant seekers), publishes tiered pricing but is a different product and is not the API surface described here.

## WebSocket

**No.** Fluxx does not publish a documented public WebSocket API. The REST API v2 is request/response REST over HTTPS with OAuth 2.0, and no `ws://`/`wss://` or server-push transport is documented. See [review.yml](review.yml).

## Tags

- Grants Management
- Grantmaking
- Nonprofit
- Philanthropy
- Foundations
- REST API

## Common Properties

- [LinkedIn](https://www.linkedin.com/company/fluxx)
- [Website](https://www.fluxx.io/)
- [GitHub Organization](https://github.com/fluxxlabs)
- [Documentation](https://www.fluxx.io/resource-center)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
