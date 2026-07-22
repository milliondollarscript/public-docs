---
slug: developer-overview
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [developers, administrators]
published: true
tags: [developers, api, extensions, wordpress]
---

# Developer Overview

Million Dollar Script 3.0 is a modular WordPress plugin with a REST API, scoped API keys, documented WordPress hooks, dynamic blocks, and extension-owned integrations. Build new integrations against these current surfaces rather than reading plugin tables or legacy post metadata directly.

## Choose an Integration Surface

| Goal | Recommended surface |
|---|---|
| Read public grids, blocks, or placements | REST API under `/wp-json/million-dollar-script/v1` |
| Manage grids, orders, pricing, or rendering | Scoped REST API key |
| Add WordPress admin or frontend behavior | Actions and filters |
| Add an editor component | `million-dollar-script/editor/extension/blocks` |
| Add a checkout provider | `MillionDollarScript\Commerce\Payments` and payment-provider filters |
| Add an extension workflow | Separate WordPress plugin with `Requires Plugins: million-dollar-script` |
| Extend migration | `million-dollar-script/migration/*` filters |

Use the [REST API Reference](/docs/mds-3/million-dollar-script/3.0.0/main/api-reference) for routes and examples. Use [Extension Development](/docs/mds-3/million-dollar-script/3.0.0/main/extension-development) for a complete plugin example.

## Runtime Requirements

- WordPress 6.0 or newer.
- PHP 8.1 or newer.
- Million Dollar Script 3.0 active.
- HTTPS for production API credentials and checkout callbacks.

Extensions should declare the WordPress dependency and MDS compatibility in the plugin header:

```php
/**
 * Plugin Name:       Million Dollar Script - Example
 * Requires at least: 6.0
 * Requires PHP:      8.1
 * Requires Plugins:  million-dollar-script
 * Requires MDS:      3.0.0
 * Requires MDS API:  1
 * MDS Generation:    3
 * MDS Compatible:    MDS 3.0
 */
```

## Namespaces and Compatibility

Extensions use the version-neutral `MillionDollarScript` public namespaces, such as `MillionDollarScript\Core\Runtime` and `MillionDollarScript\Commerce\Payments`. Other implementation namespaces are private and can change without compatibility guarantees. Selected Million Dollar Script 2 hook aliases remain available only where documented for migration; new code should use the stable hooks and facades.

Do not use `MDS3` in customer-facing labels. Display **Million Dollar Script**, **MDS**, or **MDS 3.0** when the version distinction matters.

## Stable PHP Facades

The following version-neutral classes are the supported PHP entry points for extensions. Methods not listed here may still be public in PHP terms but are not part of the extension compatibility contract.

| Domain | Class | Supported methods |
|---|---|---|
| Runtime | `MillionDollarScript\Core\Runtime` | `version()`, `api_version()`, `file()`, `path()`, `url()`, `is_ready()` |
| Settings | `MillionDollarScript\Core\Settings` | `all()`, `get()`, `defaults()`, `sanitize()`, `field_classification()`, `is_admin_visible()` |
| Grids | `MillionDollarScript\Core\Grids` | `all()`, `active()`, `find()`, `first_active()` |
| Grid value | `MillionDollarScript\Core\Grid` | `id()`, `get()`, `settings()`, `to_array()` |
| Orders | `MillionDollarScript\Core\Orders` | `create()`, `find()`, `query_for_principal()`, `find_for_principal()`, `renewal_eligibility_for_principal()`, `start_renewal_for_principal()`, `start_checkout_for_principal()`, `update_metadata()` |
| API authorization | `MillionDollarScript\Core\ApiAccess` | `authorize()`, `can_manage()` |
| Database identifiers | `MillionDollarScript\Core\Database` | `table()`, `ident()`, `table_exists()`, `column_exists()` |
| Hook compatibility | `MillionDollarScript\Core\Hooks` | `apply()`, `apply_compat()`, `do()`, `do_compat()`, `legacy_aliases()` |
| Currency | `MillionDollarScript\Commerce\Currency` | `current_code()`, `effective_code()`, `amount()`, `format()`, `normalize_code()`, `provider_locks_currency()` |
| Payments | `MillionDollarScript\Commerce\Payments` | provider discovery, checkout creation, manage URLs, and source-status methods |
| Extension registration | `MillionDollarScript\Extensions\Registry` | `register()`, `registered()` |
| Extension support | `MillionDollarScript\Extensions\Support` | admin URLs, safe redirects, REST registration, authorization, rate limiting, and browser configuration |
| Admin field UI | `MillionDollarScript\Extensions\Admin` | `description()`, `help()`, `docs_link()`, `docs_url()`, `docs_button()`, `shortcode_copy()` |
| Original media | `MillionDollarScript\Media\OriginalImage` | `resolve()` |
| Original media resolver | `MillionDollarScript\Media\OriginalAttachmentResolver` | `resolve()` |
| Customer placements | `MillionDollarScript\Media\Placements` | `update_for_principal()`, `replace_image_for_principal()` |
| Placement schedules | `MillionDollarScript\Media\PlacementSchedules` | `query()`, `find()`, `preflight()`, `set_visible()` |
| Render estimates | `MillionDollarScript\Rendering\Estimate` | `grid()`, `quota()` |

Check `Runtime::is_ready()` before calling another facade during plugin startup. Use hooks or REST for operations that do not have a facade instead of instantiating implementation classes.

## Browser Configuration

Publish extension configuration through the shared browser namespace instead of creating a global variable:

```php
\MillionDollarScript\Extensions\Support::add_browser_config(
    'mds-example-frontend',
    'example',
    'frontend',
    [
        'restUrl' => esc_url_raw(rest_url('million-dollar-script/v1/example/items')),
        'nonce' => wp_create_nonce('wp_rest'),
    ]
);
```

```js
const config = window.MillionDollarScript?.extensions?.example?.frontend ?? {};
```

`window.MillionDollarScript` is the stable browser root. Do not create standalone globals for extension configuration.

## Data Ownership

Core owns grids, blocks, placements, orders, rendering state, payment coordination, settings, setup, and API governance. Extensions should own their own tables, settings, templates, and domain records.

Avoid direct SQL against core tables. Repository schemas may evolve. Use REST endpoints, public payload filters, payment APIs, and documented hooks instead. An extension that needs its own table should use `dbDelta()`, version its schema, and retain data on deactivation. Delete data only through an explicit uninstall policy.

## Security Baseline

- Check a WordPress capability before every privileged screen or operation.
- Verify a nonce on browser form, AJAX, and REST write requests.
- Use scoped MDS API keys for non-browser integrations.
- Sanitize input at the boundary and validate relationships before saving.
- Escape values for their output context.
- Use `$wpdb->prepare()` for dynamic SQL.
- Keep private manage tokens and service signatures out of URLs and logs where possible.
- Never expose complete API keys after their one-time creation response.

## API Authentication

Create credentials under **Million Dollar Script > API Access**. Send a key with either header:

```http
Authorization: Bearer milliondollarscript_your_key
```

```http
X-Million-Dollar-Script-API-Key: milliondollarscript_your_key
```

API keys and extension-server tester access keys are different credentials. API keys authorize a client against one WordPress site. Tester access keys authorize extension catalog, package, update, and private-documentation access through the extension server.

## Discovery

Authenticated clients can inspect:

- `/wp-json/million-dollar-script/v1/extensions/discovery` for normalized endpoint and extension capabilities.
- `/wp-json/million-dollar-script/v1/extensions/openapi` for the generated OpenAPI 3.1 document.

The static API reference remains usable without access to a WordPress installation. Discovery is useful when an active extension adds routes or schemas at runtime.

## Backward Compatibility

MDS 2 documentation remains available through the version selector. Code that reads `mds-pixel` posts, Carbon Fields values, legacy `wp_mds_*` tables, or old list/menu hooks should follow the [MDS 2 upgrade guide](/docs/mds-3/million-dollar-script/3.0.0/main/mds3-upgrade-from-mds2) and [developer upgrade notes](/docs/mds-3/million-dollar-script/3.0.0/main/developer-upgrade-notes).
