---
slug: developer-upgrade-notes
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

Million Dollar Script 3.0 retains selected compatibility surfaces while moving new integrations to scoped REST APIs, repositories, extension-owned settings, and payment-provider adapters.

## Naming and Runtime

Use **Million Dollar Script** in user-facing copy. Extensions should use the version-neutral `MillionDollarScript` public namespaces. Use `million-dollar-script/loaded` for new load integrations. The Million Dollar Script 2 actions `mds_initialized`, `mds_loaded`, and `mds-loaded` remain silent compatibility aliases for side-by-side migration.

Replace compatibility-only access with the supported facade that owns the operation:

```php
// Before: ambiguous global shared with older plugin generations.
$version = MDS_VERSION;

// Current: runtime facade owned by the active core plugin.
$version = \MillionDollarScript\Core\Runtime::version();
```

| Replace | With |
|---|---|
| Direct plugin implementation classes | A documented `MillionDollarScript\Core`, `Commerce`, `Extensions`, `Media`, or `Rendering` facade |
| Million Dollar Script 2 extension helpers | A `MillionDollarScript\Extensions` facade or a documented hook |
| `MDS_VERSION`, `MDS_FILE`, `MDS_PATH`, or `MDS_URL` | `MillionDollarScript\Core\Runtime` methods |
| `mds_initialized`, `mds_loaded`, or `mds-loaded` | `million-dollar-script/loaded` |
| Extension-owned browser globals | `window.MillionDollarScript.extensions` through `MillionDollarScript\Extensions\Support::add_browser_config()` |

## Custom Fields

Migration reads active definitions from the legacy `mds_custom_fields` table and keeps recognized values in order metadata under `mds_fields`. Other non-core ad metadata is retained under `legacy_ad_post_meta`.

Field definitions are not automatically created in the Fields extension. Recreate fields with matching keys when migrated values must remain visible. Migration integrations may use `million-dollar-script/migration/legacy/mds/field/definitions`, `million-dollar-script/migration/legacy/mds/fields`, `million-dollar-script/migration/legacy/ad/metadata`, and `million-dollar-script/migration/legacy/order/metadata`.

## Payments

New monetization extensions call `MillionDollarScript\Commerce\Payments` instead of WooCommerce, Stripe, Easy Digital Downloads, or another checkout system directly. The active provider extension owns gateway-specific behavior.

## Settings

Compatibility-only settings remain importable when needed but may be hidden from the current Settings UI. Extensions should own their settings and add setup or navigation actions through documented hooks.

## REST Authentication

Use `/wp-json/million-dollar-script/v1` with `Authorization: Bearer ...` or `X-Million-Dollar-Script-API-Key`. Browser writes governed by the public-write nonce policy also require `X-WP-Nonce`. Development-only REST namespaces are not registered.

## Upgrade Checklist

1. Replace direct legacy table and post-meta reads with current APIs or hooks.
2. Replace MDS 2 menu registration and list-page hooks with current extension onboarding, dashboard, block, and REST integrations.
3. Move gateway-specific calls behind the core payments layer.
4. Recreate custom-field definitions with stable keys.
5. Test migration on a staging copy with a dry run and backup.
6. Verify MDS 2 and MDS 3.0 packages are filtered to their matching core generation.
