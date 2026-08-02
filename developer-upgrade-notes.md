---
slug: developer-upgrade-notes
summary: Upgrade custom integrations safely with current public APIs, migration hooks, payment facades, settings ownership, and extension UI hooks.
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [developers, administrators]
published: true
tags: [developers, migration, compatibility, mds2]
---

# Developer Upgrade Notes

Million Dollar Script 3.0 keeps selected compatibility surfaces for Million Dollar Script 2 migrations while moving new integrations to version-neutral public APIs, extension-owned settings, and payment-provider adapters.

## Naming and Runtime

Use **Million Dollar Script** in user-facing copy. Use **MDS** only when the context is already clear, and use **MDS 3.0** when the version distinction matters.

Extensions should use the public namespaces under `MillionDollarScript\Core`, `MillionDollarScript\Commerce`, `MillionDollarScript\Extensions`, `MillionDollarScript\Media`, and `MillionDollarScript\Rendering`. Do not depend on internal implementation namespaces or classes.

Million Dollar Script 3.0 does not define the ambiguous `MDS_VERSION`, `MDS_FILE`, `MDS_PATH`, `MDS_URL`, or `MDS_BASENAME` globals used by Million Dollar Script 2. Read runtime metadata through `MillionDollarScript\Core\Runtime` so both plugin generations can load in either order.

```php
// Before: ambiguous global shared with older plugin generations.
$version = MDS_VERSION;

// Current: runtime facade owned by the active core plugin.
$version = \MillionDollarScript\Core\Runtime::version();
```

Use `million-dollar-script/loaded` for new load integrations. The older `mds_initialized`, `mds_loaded`, and `mds-loaded` actions remain silent compatibility aliases for side-by-side Million Dollar Script 2 migration.

| Replace | With |
|---|---|
| Direct plugin implementation classes | A documented `MillionDollarScript` public facade |
| Million Dollar Script 2 extension helpers | A `MillionDollarScript\Extensions` facade or documented hook |
| `MDS_VERSION`, `MDS_FILE`, `MDS_PATH`, `MDS_URL`, or `MDS_BASENAME` | `MillionDollarScript\Core\Runtime` methods |
| `mds_initialized`, `mds_loaded`, or `mds-loaded` | `million-dollar-script/loaded` |
| Extension-owned browser globals | `window.MillionDollarScript.extensions` through `MillionDollarScript\Extensions\Support::add_browser_config()` |

## Browser Integrations

Use the shared `window.MillionDollarScript` namespace instead of creating standalone globals. Interactive grid instances are available through `window.MillionDollarScript.gridInstances`. Extension configuration registered with `MillionDollarScript\Extensions\Support::add_browser_config()` is published under `window.MillionDollarScript.extensions`.

## Custom Fields and Migrated Data

Migration reads active definitions from the legacy `mds_custom_fields` table and keeps recognized values in order metadata under `mds_fields`. Other non-core ad metadata is retained under `legacy_ad_post_meta`.

Field definitions are not automatically created in the Fields extension. Recreate fields with matching keys when migrated values must remain visible. Code that reads old order, pixel, page, or `_milliondollarscript_<field_key>` post metadata directly should move to current order metadata, repositories, REST endpoints, shortcodes, filters, or documented compatibility hooks.

Migration integrations may extend the mapping with:

- `million-dollar-script/migration/legacy/mds/field/definitions`
- `million-dollar-script/migration/legacy/mds/fields`
- `million-dollar-script/migration/legacy/ad/metadata`
- `million-dollar-script/migration/legacy/order/metadata`

Placement-field extensions can use `million-dollar-script/placement/form/fields` for both the interactive grid and the Manage Upload screen. The first argument is the grid object on an interactive grid and `null` on Manage Upload. The optional second argument is a context array; Manage Upload provides `context`, `order`, and `placement` values. Callbacks registered for one argument continue to work.

## Payments

Monetization extensions should call `MillionDollarScript\Commerce\Payments` instead of calling WooCommerce, Easy Digital Downloads, Stripe, or another checkout system directly. The active provider extension owns gateway-specific behavior, while the core payments facade exposes provider discovery, checkout creation, recurring-payment operations, and source status updates.

Use the core currency facade or the active provider rather than hardcoding currency labels or assumptions.

## Settings Compatibility

Some Million Dollar Script 2 settings remain available for migration, import/export, or custom-code compatibility but are hidden from the primary Settings interface when they do not affect the current runtime. Extensions should own settings and rendering controls for extension-owned features.

Use `MillionDollarScript\Core\Settings::field_classification($key)` to determine whether a setting is active, compatibility-only, deferred, or extension-owned. Use `MillionDollarScript\Core\Settings::is_admin_visible($key)` before presenting a core setting in custom admin UI.

Settings import/export uses the full schema, including hidden compatibility fields. An extension that needs package-owned settings included in transfer payloads can use `million-dollar-script/admin/settings/transfer/fields`.

## Extension Navigation and Presentation

Extensions should register setup, settings, and documentation actions through current APIs instead of modifying the WordPress admin menu DOM.

- Use `million-dollar-script/extension/onboarding/items` for the extension's primary setup or settings action.
- Use `million-dollar-script/dashboard/extension/cards` to customize a dashboard card, add actions, or link to documentation.
- Use `million-dollar-script/admin/bar/extension/items` only when the admin-bar action should differ from the primary onboarding action.
- Use `million-dollar-script/extension/visual/metadata` for supported icon and accent-color metadata.
- Use `million-dollar-script/dashboard/paid/revenue/url` when an active reporting extension should link the Paid revenue metric to a dedicated report.

MDS-owned extensions may also declare `MDS Icon` and `MDS Accent Color` plugin headers. Icons should use a Dashicons class or slug, and accent colors should use six-digit hexadecimal notation.

Core owns the scoped Million Dollar Script sidebar grouping. Register extension admin pages only while the extension is active, keep user-facing labels free of internal shorthand, and use package-owned documentation manifests for bundled docs.

## REST Authentication

Use the `/wp-json/million-dollar-script/v1` namespace with `Authorization: Bearer ...` or `X-Million-Dollar-Script-API-Key`. Browser write endpoints governed by the public-write nonce policy also require a valid `X-WP-Nonce`. Development-only REST namespaces are not registered in release builds.

## Upgrade Checklist

1. Replace direct implementation-class, legacy table, and post-meta access with current public APIs or documented hooks.
2. Replace Million Dollar Script 2 menu and list-page integrations with current onboarding, dashboard, block, and REST integrations.
3. Move gateway-specific behavior behind `MillionDollarScript\Commerce\Payments`.
4. Recreate custom-field definitions with the same stable keys when migrated values must remain visible.
5. Move extension configuration into the shared browser namespace and package-owned settings.
6. Test migration on a staging copy with a dry run and a verified backup.
7. Confirm packages, updates, and documentation are filtered to the matching Million Dollar Script generation.
