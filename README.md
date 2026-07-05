# Fluxx (fluxx-grants)

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
