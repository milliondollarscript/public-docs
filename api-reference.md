---
slug: api-reference
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [developers, administrators]
published: true
tags: [api, rest, authentication, developers]
summary: Use the versioned Million Dollar Script REST API, authentication scopes, grid and order resources, extension discovery, and secure integration patterns.
---

# REST API Reference

Million Dollar Script exposes WordPress REST routes under:

```text
https://example.com/wp-json/million-dollar-script/v1
```

This static reference covers core routes without requiring a plugin installation. Active extensions can add routes at runtime; use authenticated discovery for the complete site-specific contract.

Download the [static OpenAPI 3.1 document](/assets/mds-3.0-openapi.json) for code generation and contract tooling. Its server variable must be replaced with the WordPress site where Million Dollar Script is installed.

## Authentication

Public-read routes return only fields intended for anonymous display. Other routes use the authentication method declared by their effective endpoint policy. Most integration routes use scoped API keys; browser nonce, resource-owned manage-token, service-signature, and administrator routes use their own boundaries.

```http
Authorization: Bearer milliondollarscript_your_key
```

or:

```http
X-Million-Dollar-Script-API-Key: milliondollarscript_your_key
```

Create and rotate keys under **Million Dollar Script > API Access**. The complete key is returned only when it is created or rotated.

Keep API keys in a server-side secret store or environment variable. Do not embed them in browser JavaScript, public repositories, support logs, analytics events, or URLs. Browser code should call only anonymous public-read routes or a server-side integration that enforces its own authorization.

Browser writes configured for `public_write_nonce` require a valid WordPress REST nonce in `X-WP-Nonce`. WordPress administrator routes require an authenticated administrator session and cannot be downgraded to API-key access.

## Security Levels

| Level | Meaning |
|---|---|
| `public_read` | Anonymous read of deliberately public fields |
| `public_write_nonce` | Browser write with a valid WordPress REST nonce |
| `api_key_read` | Scoped API key with read permission |
| `api_key_write` | Scoped API key with write permission |
| `signed_manage_token` | Endpoint-owned private manage token or authorized owner; otherwise administrator |
| `service_signature` | Exact trusted service verifier registered by the owning extension, or administrator |
| `wp_capability` | Authenticated WordPress administrator |
| `disabled` | Endpoint blocked by policy |

An administrator may strengthen or disable an endpoint policy, but cannot weaken its minimum security level.

## Service-Signature Version 1

`service_signature` authenticates an extension-owned server identity. A service ID, label, endpoint URL, header, or ordinary API key is not sufficient. Core accepts an authenticated WordPress administrator for local administration; otherwise it validates the common envelope and invokes only the verifier registered for the exact endpoint ID, scope, service ID, and signature version. Missing verifiers and every non-`true` result fail closed.

Send these headers over HTTPS:

```text
X-MDS-Service-Id
X-MDS-Signature-Version: v1
X-MDS-Timestamp
X-MDS-Nonce
X-MDS-Content-SHA256
X-MDS-Signature
X-Idempotency-Key          # only when the endpoint uses it
```

`X-MDS-Timestamp` is a ten-digit Unix timestamp no more than 300 seconds in the past or future. `X-MDS-Nonce` is the 43-character unpadded base64url encoding of 32 cryptographically random bytes. The content hash is lowercase hexadecimal SHA-256 of the exact raw request body. The signature is lowercase hexadecimal HMAC-SHA256.

Build the canonical string by joining these eight values with a single LF and no trailing LF:

```text
v1
<SERVICE_ID>
<UPPERCASE_METHOD>
<CANONICAL_ROUTE>
<TIMESTAMP>
<NONCE>
<BODY_SHA256>
<IDEMPOTENCY_KEY_OR_EMPTY_STRING>
```

The canonical route starts at `/million-dollar-script/v1/`; omit scheme, host, `/wp-json`, query, and fragment. Do not parse and re-serialize JSON after hashing it. Compare signatures in constant time.

```text
body_hash = hex_sha256(exact_body_bytes)
nonce = base64url(random_bytes(32), no_padding)
canonical = join_with_lf("v1", service_id, upper(method), route,
                         timestamp, nonce, body_hash, idempotency_key_or_empty)
signature = hex_hmac_sha256(secret, canonical)
```

The owning extension stores credentials and must enforce status, expiry, scope, revocation, domain-object ownership, rate limits, and an atomic nonce claim retained for at least the accepted timestamp window. Use a new nonce for every attempt; reuse only the endpoint's idempotency key when retrying the same logical operation.

Invalid, unknown, expired, revoked, replayed, malformed, or tampered requests return the same generic `401`. HTTPS failures use `403`, rate limits use `429`, and a privacy-safe temporary verifier failure may use `503`. Responses and authorization audits never include the secret, expected signature, signature header, nonce, or raw body.

The protocol protects request integrity and replay within its window; it cannot protect a copied secret or a compromised signing or WordPress host. Keep secrets in server-side secret storage, rotate exposed credentials, and retain endpoint domain validation and local moderation.

## Response and Error Format

Successful responses use JSON. WordPress REST errors use this shape:

```json
{
  "code": "mds3_grid_not_found",
  "message": "Grid not found.",
  "data": {"status": 404}
}
```

Common status codes are `400` for invalid input, `401` for missing or invalid authentication, `403` for policy or scope denial, `404` for missing resources, `429` for a key rate limit, and `500` for an unexpected operation failure.

## Grid Routes

| Method | Route | Scope | Minimum security | Purpose |
|---|---|---|---|---|
| `GET` | `/grids` | `core.grid.read` | `public_read` | List active public grids |
| `POST` | `/grids` | `core.grid.write` | `api_key_write` | Create a grid |
| `GET` | `/grids/{id}` | `core.grid.read` | `public_read` | Read an active grid |
| `PUT`, `PATCH` | `/grids/{id}` | `core.grid.write` | `api_key_write` | Update a grid |
| `DELETE` | `/grids/{id}` | `core.grid.write` | `api_key_write` | Archive a grid |
| `GET` | `/grids/{id}/blocks` | `core.grid.read` | `public_read` | List block geometry and status |
| `POST` | `/grids/{id}/availability` | `core.grid.write` | `api_key_write` | Update a rectangular availability region |
| `GET` | `/grids/{id}/packages` | `core.grid.read` | `api_key_read` | List pricing packages |
| `POST` | `/grids/{id}/packages` | `core.grid.write` | `api_key_write` | Create or update a package |
| `GET` | `/grids/{id}/price-rules` | `core.grid.read` | `api_key_read` | List price-zone rules |
| `POST` | `/grids/{id}/price-rules` | `core.grid.write` | `api_key_write` | Create or update a price rule |

A public grid payload includes `id`, `slug`, `title`, `description`, dimensions, block dimensions, base price, currency, status, renderer mode, and virtual row, column, and block totals. Administrator-authenticated reads may include additional internal fields.

```bash
curl https://example.com/wp-json/million-dollar-script/v1/grids
```

Public grid discovery can run in browser JavaScript without exposing a key:

```js
const response = await fetch('https://example.com/wp-json/million-dollar-script/v1/grids', {
  headers: { Accept: 'application/json' },
});

if (!response.ok) {
  throw new Error(`Grid discovery failed with HTTP ${response.status}`);
}

const grids = await response.json();
```

## Placement and Reservation Routes

| Method | Route | Scope | Minimum security | Purpose |
|---|---|---|---|---|
| `GET` | `/grids/{id}/placements` | `core.placement.read` | `public_read` | List active public placements |
| `POST` | `/grids/{id}/placements` | `core.placement.write` | `api_key_write` | Create a placement from an existing WordPress attachment |
| `POST` | `/grids/{id}/reservations` | `core.order.write` | `api_key_write` | Reserve selected blocks and create checkout state |

Reservation input accepts a `blocks` array and may include `email`, `user_id`, `package_id`, and extension metadata. Each block identifies a zero-based grid `row` and `col`, not a pixel rectangle. Coordinates must be valid for the selected grid and pass availability, adjacency, overlap, and order-limit validation.

```json
{
  "blocks": [
    {"row": 0, "col": 0},
    {"row": 0, "col": 1}
  ],
  "email": "buyer@example.com",
  "package_id": 4,
  "metadata": {"campaign": "summer"}
}
```

Create a reservation from a trusted server process:

```bash
curl -X POST \
  -H "Authorization: Bearer $MDS_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{"blocks":[{"row":0,"col":0},{"row":0,"col":1}],"email":"buyer@example.com"}' \
  https://example.com/wp-json/million-dollar-script/v1/grids/1/reservations
```

The placement creation route accepts an existing WordPress Media Library `attachment_id`; it is not a raw file-upload endpoint. Upload and validate media through WordPress first, then create the placement with the returned attachment ID and the intended order, block, geometry, fit mode, destination URL, alternative text, and popup text. The site-wide **Advertiser URL Field** and **Popup Text Field** settings also govern REST writes: required fields reject blank values, optional fields accept them, and hidden fields ignore submitted values.

Public placement responses include safe geometry, fit mode, destination URL, alternative text, popup text, status, source-image dimensions and MIME type, and the placement mask. Private customer details and raw order metadata are not included in anonymous responses.

## Order Routes

| Method | Route | Scope | Minimum security | Purpose |
|---|---|---|---|---|
| `GET` | `/orders?per_page=50` | `core.order.read` | `api_key_read` | List up to 100 recent orders |
| `GET` | `/orders/{id}` | `core.order.read` | `api_key_read` | Read an order, its items, and placement rectangle |
| `PUT`, `PATCH` | `/orders/{id}` | `core.order.write` | `api_key_write` | Update order status |
| `GET` | `/orders/{id}/items` | `core.order.read` | `api_key_read` | Read order items |

Supported status updates are `reserved`, `pending_payment`, `paid`, `cancelled`, `failed`, `refunded`, `expired`, `denied`, and `deleted`. Setting `paid` also asks the active payment provider to complete its associated order.

```bash
curl -X PATCH \
  -H 'Authorization: Bearer milliondollarscript_your_key' \
  -H 'Content-Type: application/json' \
  -d '{"status":"paid"}' \
  https://example.com/wp-json/million-dollar-script/v1/orders/42
```

## Rendering Routes

| Method | Route | Scope | Minimum security | Purpose |
|---|---|---|---|---|
| `GET` | `/grids/{id}/render-status` | `core.render.read` | `api_key_read` | Read current render state |
| `GET` | `/grids/{id}/render-preflight` | `core.render.read` | `api_key_read` | Inspect render preflight |
| `POST` | `/grids/{id}/render-preflight` | `core.render.write` | `api_key_write` | Run render preflight |
| `POST` | `/grids/{id}/render` | `core.render.write` | `api_key_write` | Submit a render job |
| `GET` | `/render-jobs/{id}` | `core.render.read` | `api_key_read` | Read one render job |

ImageGrid account management appears at `/imagegrid/account` only while the ImageGrid extension is active. It requires a WordPress administrator.

## Extension Discovery

| Method | Route | Scope | Minimum security | Purpose |
|---|---|---|---|---|
| `GET` | `/extensions` | `core.extension.read` | `api_key_read` | Read the compatible extension catalog |
| `GET` | `/extensions/capabilities` | `core.extension.read` | `api_key_read` | Read active extension capabilities |
| `GET` | `/extensions/setup` | `core.extension.read` | `api_key_read` | Read setup choices supplied by extensions |
| `GET` | `/extensions/discovery` | `core.extension.read` | `api_key_read` | Read normalized endpoint and package metadata |
| `GET` | `/extensions/openapi` | `core.extension.read` | `api_key_read` | Read the OpenAPI 3.1 contract |

Discovery omits malformed or incomplete endpoint declarations. It does not assign permissive defaults to unknown routes.

## API Administration

These routes require an authenticated WordPress administrator and are intended for the API Access screen or trusted administrative tools:

| Method | Route | Purpose |
|---|---|---|
| `GET` | `/api/discovery` | Read administrator discovery data |
| `GET`, `POST` | `/api/keys` | List active keys or create a key |
| `DELETE` | `/api/keys/{id}` | Revoke a key immediately |
| `POST` | `/api/keys/{id}/rotate` | Replace a key and return the new secret once |
| `GET` | `/api/audit-logs` | Read recent allow and deny decisions |

## Migration Routes

| Method | Route | Scope | Minimum security | Purpose |
|---|---|---|---|---|
| `POST` | `/migration/dry-run` | `core.migration.write` | `wp_capability` | Inspect the available Million Dollar Script 2 migration without writing data |
| `POST` | `/migration/execute` | `core.migration.write` | `wp_capability` | Import the reviewed Million Dollar Script 2 data |

Run a dry run first, review warnings and row counts, create a backup, and execute only when the source prefix and migration choice are correct.

## Server-side PHP Example

This example reads endpoint discovery from another WordPress plugin without placing the key in a URL or browser bundle:

```php
$api_key = getenv('MDS_INTEGRATION_API_KEY');
$response = wp_remote_get(
    'https://example.com/wp-json/million-dollar-script/v1/extensions/discovery',
    [
        'headers' => [
            'Accept' => 'application/json',
            'Authorization' => 'Bearer ' . $api_key,
        ],
        'timeout' => 15,
    ]
);

if (is_wp_error($response)) {
    return $response;
}

$status = wp_remote_retrieve_response_code($response);
if (200 !== $status) {
    return new WP_Error('mds_integration_http_error', 'Million Dollar Script discovery failed.', ['status' => $status]);
}

$discovery = json_decode(wp_remote_retrieve_body($response), true, 512, JSON_THROW_ON_ERROR);
```

Store the environment variable outside the web root and never log `$api_key` or full request headers.

## Write Safety And Retries

Core reservation creation is not an idempotent operation. Do not retry a timed-out reservation blindly: query recent orders, correlate your own non-secret metadata, and confirm whether the first request created an order. Extension endpoints may advertise their own idempotency contract through discovery and OpenAPI metadata.

Use `PATCH` for an intentional order-state transition, verify the returned order, and treat `409`, `429`, and `5xx` responses as outcomes that require inspection before another write. Do not rotate keys to bypass a rate limit.

## Versioning And Extension Contracts

The `/million-dollar-script/v1` namespace is the compatibility boundary for this API generation. Additive response fields and new endpoints may appear without changing the namespace; clients must ignore unknown fields. Breaking request or response changes require a new namespace or an explicit deprecation period.

For PHP service and callback integrations, see [Developer Overview](/docs/mds-3/million-dollar-script/3.0.0/main/developer-overview), [Extension Development](/docs/mds-3/million-dollar-script/3.0.0/main/extension-development), and [Hooks Reference](/docs/mds-3/million-dollar-script/3.0.0/main/hooks-reference). Payment providers report source status through the documented payment actions. Optional extensions can add endpoints and schemas to runtime discovery without exposing private implementation details in the static core document.

## Pagination and Rate Limits

The order list accepts `per_page` from 1 to 100. Other collection routes currently return the available collection for their resource. Clients should tolerate future pagination fields and ignore unknown response properties.

Each API key has an hourly request limit. On `429`, pause requests and retry after the next allowed interval. Do not rotate keys to bypass limits.

## Compatibility Rules

- Treat numeric IDs as opaque identifiers even when responses encode them as JSON numbers.
- Send UTF-8 JSON with `Content-Type: application/json` for writes.
- Do not depend on undocumented database columns or response ordering.
- Ignore unknown response fields so extensions can add compatible metadata.
- Use endpoint discovery before calling routes supplied by optional extensions.
- Send API keys only through `Authorization: Bearer ...` or `X-Million-Dollar-Script-API-Key`.
