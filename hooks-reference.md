---
slug: hooks-reference
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [developers, administrators]
published: true
tags: [developers, hooks, filters, actions]
---

# Hooks Reference

This reference lists the primary MDS 3.0 extension hooks. Callback arguments shown here are part of the supported integration surface. Return the original value when a filter does not apply to your extension.

Unless a section says otherwise, these hooks are available from MDS 3.0.0 and are not deprecated. Filters may change only the value they receive and must return the documented type. Actions do not use return values. Sanitize data before persistence, escape it at the final output boundary, and do not weaken a capability, nonce, API scope, or access decision made by core.

New integrations must use the `million-dollar-script/...` names below. Core does not dispatch aliases created during unreleased development. Selected Million Dollar Script 2 aliases remain only where this reference explicitly documents them for migration.

## Extension Registration and Navigation

### `million-dollar-script/extension/onboarding/items`

```php
function (array $items, array $context): array
```

Register an active extension's name, summary, setup actions, recommended pages, and legal-document drafts. Core uses the result in setup, dashboard cards, sidebar grouping, and admin-bar navigation.

### `million-dollar-script/dashboard/extension/cards`

```php
function (array $cards, array $catalog, object $admin): array
```

Customize dashboard cards for active extensions. Prefer onboarding items unless the card needs additional status or actions.

### `million-dollar-script/admin/bar/extension/items`

Filter extension shortcuts in the WordPress admin bar. Use it only when the destination differs from the primary onboarding action.

### `million-dollar-script/extension/visual/metadata`

Filter the icon and accessible accent metadata associated with an extension card or catalog entry.

## Editor Blocks

### `million-dollar-script/editor/extension/blocks`

```php
function (array $definitions): array
```

Add dynamic editor blocks. Each definition should include `name`, `title`, `description`, `icon`, `attributes`, `controls`, `preview`, and `render_callback`. Core validates and registers the definition.

## Payments and Currency

### `million-dollar-script/payment/provider/options`

```php
function (array $options): array
```

Add a provider choice to setup and settings.

### `million-dollar-script/payment/providers`

```php
function (array $providers): array
```

Register provider readiness, checkout, completion, and currency callbacks. Monetized extensions should call `MillionDollarScript\Commerce\Payments` rather than provider plugins directly.

### `million-dollar-script/payments/pre/checkout/payload`

```php
function (array $payload, array $transaction, array $provider): array
```

Adjust normalized checkout input before the provider callback.

### `million-dollar-script/payments/checkout/payload`

```php
function (array $payload, array $transaction, array $provider): array
```

Adjust the checkout response returned to the source extension.

### Payment status actions

```php
do_action('million-dollar-script/payment/source/paid', string $source, int $source_id, array $context);
do_action('million-dollar-script/payment/source/cancelled', string $source, int $source_id, array $context);
do_action('million-dollar-script/payment/source/released', string $source, int $source_id, string $status, array $context);
do_action('million-dollar-script/payment/source/status', string $source, int $source_id, string $status, array $context);
```

Check `$source` before updating extension-owned records.

## Placement Forms and Public Grids

### `million-dollar-script/placement/form/fields`

```php
function ($grid = null, array $context = []): void
```

Render extension-owned placement fields. Interactive grids pass the grid object. The Manage Upload screen passes `null` plus `context`, `order`, and `placement`. One-argument callbacks remain compatible.

### `million-dollar-script/validate/placement/submission`

Filter placement validation results before a submission is saved. Return a `WP_Error` for invalid extension fields without exposing sensitive validation details.

### `million-dollar-script/placement/saved`

Runs after placement data is stored. Use it to persist extension-owned values linked to the placement or order.

### Public payload filters

| Filter | Arguments | Purpose |
|---|---|---|
| `million-dollar-script/public/grid/payload` | `$payload, $grid, $settings` | Adjust the frontend AJAX grid payload |
| `million-dollar-script/rest/public/grid/payload` | `$payload, $grid` | Adjust the anonymous REST grid payload |
| `million-dollar-script/placement/payload` | `$payload, $placement, $settings` | Add safe placement fields to frontend data |
| `million-dollar-script/placement/popover/html` | `$html, $payload, $settings` | Adjust sanitized placement popover markup |
| `million-dollar-script/popup/text/allowed/html` | `$allowed_html` | Extend the allowlist used for popup content |

Never add private order, customer, manage-token, or unescaped HTML fields to public payloads.

### Individual advertiser pages

| Hook | Arguments | Purpose |
|---|---|---|
| `million-dollar-script/advertiser/page/view-model` | `$model, $placement_id` | Add a minimal, sanitized public view model before rendering and metadata filters |
| `million-dollar-script/advertiser/page/before` | `$model` | Render before the standard advertiser-page content |
| `million-dollar-script/advertiser/page/before-image` | `$model` | Render before the placement image |
| `million-dollar-script/advertiser/page/after-image` | `$model` | Render after the placement image |
| `million-dollar-script/advertiser/page/before-content` | `$model` | Render before the placement description |
| `million-dollar-script/advertiser/page/after-content` | `$model` | Render after the placement description |
| `million-dollar-script/advertiser/page/before-actions` | `$model` | Render before the advertiser and grid links |
| `million-dollar-script/advertiser/page/after-actions` | `$model` | Render after the advertiser and grid links |
| `million-dollar-script/advertiser/page/after` | `$model` | Render at the end of the advertiser page article |

The view-model filter is public output. Add only values intentionally approved for that placement surface. Do not attach raw orders, customer records, payment data, manage credentials, or unpublished extension settings. Theme overrides of `million-dollar-script/single-advertiser.php` should preserve the standard action hooks when extension content is expected to render.

## REST API Governance

### `million-dollar-script/api/endpoint/manifest`

```php
function (array $endpoints): array
```

Register extension REST endpoints for policy management and discovery. Each item needs:

```php
[
    'id' => 'example-items-read',
    'route' => '/million-dollar-script/v1/example/items',
    'methods' => ['GET'],
    'scope' => 'example.item.read',
    'minimum_security_level' => 'api_key_read',
    'description' => __('List Example items.', 'mds-example'),
]
```

Valid public policy values are `public_read`, `public_write_nonce`, `api_key_read`, `api_key_write`, `wp_capability`, and `disabled`.

### `million-dollar-script/api/openapi/document`

```php
function (array $document, object $governance): array
```

Add schemas, examples, or tags to the OpenAPI 3.1 document after endpoint normalization.

### `million-dollar-script/admin/api/key/scope/options`

Add named scopes to the API Access screen. Scope labels should explain the exact data and operations granted.

## Grid and Order Administration

| Hook | Purpose |
|---|---|
| `million-dollar-script/admin/grid/tabs` | Add a grid-edit tab |
| `million-dollar-script/admin/grid/list/extra/columns` | Declare an extra grid-list column |
| `million-dollar-script/admin/grid/list/column/html` | Render sanitized content for an extra column |
| `million-dollar-script/admin/grid/list/row/actions` | Add a capability-checked row action |
| `million-dollar-script/admin/grid/saved` | React after a grid is saved |
| `million-dollar-script/admin/validate/grid` | Return grid validation errors before save |
| `million-dollar-script/order/placement/moved` | React after an administrator moves an order placement |
| `million-dollar-script/order/renewal/started` | React after renewal checkout is created |

`million-dollar-script/order/placement/moved` receives the order ID, destination grid ID, move summary, and context. Do not duplicate core block-release or overlap handling in an extension.

## Settings

| Filter | Purpose |
|---|---|
| `million-dollar-script/admin/settings/tabs` | Add a settings tab owned by an active extension |
| `million-dollar-script/admin/settings/groups` | Add a settings group |
| `million-dollar-script/settings/field/schema` | Extend a known field schema |
| `million-dollar-script/settings/help` | Supply concise field help text |
| `million-dollar-script/admin/sanitize/settings` | Sanitize extension values during save |
| `million-dollar-script/admin/validate/settings` | Return validation errors before persistence |
| `million-dollar-script/admin/settings/transfer/fields` | Include extension-owned values in import/export |
| `million-dollar-script/admin/settings/saved` | React after settings persist |
| `million-dollar-script/admin/settings/imported` | React after a settings import |

Prefer a separate extension settings screen when a feature needs several sections, reports, or operational records. Use a core settings tab only for a small, directly related set of fields.

## Email Notifications

| Hook | Purpose |
|---|---|
| `million-dollar-script/order/notification/definitions` | Add or adjust notification definitions |
| `million-dollar-script/order/notification/recipients` | Filter recipient addresses |
| `million-dollar-script/order/notification/subject` | Filter a plain-text subject |
| `million-dollar-script/order/notification/message` | Filter the HTML message |
| `million-dollar-script/order/notification/headers` | Filter mail headers |
| `million-dollar-script/order/notification/placeholder/values` | Add safe template placeholders |
| `million-dollar-script/order/notification/type/for/status` | Map an order transition to a notification type |
| `million-dollar-script/order/notification/sent` | Observe each delivery result |

Million Dollar Script sends through `wp_mail()`. Do not assume a specific SMTP or logging plugin.

## Documentation Packages

### `million-dollar-script/docs/manifest/paths`

```php
function (array $paths, object $registry): array
```

Register an extension's bundled manifest path when local package documentation is appropriate.

### `million-dollar-script/docs/packages`

```php
function (array $packages, object $registry): array
```

Add normalized local or remote documentation packages. Paid extension bodies must remain entitlement-gated by the documentation service.

## Migration

| Filter | Purpose |
|---|---|
| `million-dollar-script/migration/legacy/mds/field/definitions` | Extend imported field definitions |
| `million-dollar-script/migration/legacy/mds/fields` | Map recognized ad field values |
| `million-dollar-script/migration/legacy/ad/metadata` | Preserve or transform non-core ad metadata |
| `million-dollar-script/migration/legacy/order/metadata` | Preserve or transform order metadata |

Migration filters should be deterministic and idempotent because an administrator may run multiple dry runs before importing.

## Load Hooks

`million-dollar-script/loaded` is the current load action. The older `mds_initialized`, `mds_loaded`, and `mds-loaded` actions remain silent compatibility aliases for side-by-side Million Dollar Script 2 migration. New extensions should use `plugins_loaded` with a later priority or `million-dollar-script/loaded` and declare the WordPress plugin dependency.
