---
slug: mds3-upgrade-from-mds2
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [site-owners, administrators]
published: true
---

# Upgrade From Million Dollar Script 2

Million Dollar Script is designed so existing Million Dollar Script 2 site owners can install it beside the old plugin, review the migration, and choose when to move. Million Dollar Script does not automatically import Million Dollar Script 2 data or deactivate Million Dollar Script 2.

For a feature-by-feature view of what is covered and what still needs validation for a live site, see [Million Dollar Script 2 Workflow Parity](/docs/mds-3/million-dollar-script/3.0.0/main/mds3-mds2-workflow-parity).

## Before You Start

- Back up the WordPress database and `wp-content/uploads`.
- Confirm the current Million Dollar Script 2 site works before changing anything.
- Keep the Million Dollar Script 2 plugin ZIP available in case you want to roll back.
- If WooCommerce is used for payments, confirm WooCommerce orders and checkout are healthy first.

## Install Million Dollar Script

1. Go to WordPress Admin > Plugins > Add New > Upload Plugin.
2. Upload the Million Dollar Script ZIP.
3. Activate Million Dollar Script.
4. WordPress redirects you to Million Dollar Script > Setup.

If Million Dollar Script 2 is installed, Million Dollar Script shows a "Million Dollar Script 2 Upgrade Choice" step. This step detects the old plugin package and the legacy `wp_mds_*` tables. No migration runs until you choose an action.

## Your Upgrade Choices

### Keep Million Dollar Script 2 Active For Now

Choose this when you want to compare Million Dollar Script 2 and Million Dollar Script side by side.

- Million Dollar Script 2 remains active.
- Million Dollar Script does not import old data.
- Million Dollar Script does not take over the old `[milliondollarscript]` embeds while Million Dollar Script 2 is active.
- Use Million Dollar Script pages and shortcodes for new testing.

This is useful for review, but not recommended long term because both plugins manage similar admin menus, checkout concepts, and grid pages.

### Review Migration Dry Run

Open the migration dry run before importing. The dry run reports:

- Million Dollar Script 2 source table prefix
- Million Dollar Script 2 tables found and row counts
- page-wizard pages and shortcode/block pages
- Million Dollar Script 2 options that can be mapped
- target Million Dollar Script tables
- imported, skipped, repaired, and warning totals
- anonymous source record identifiers and reasons for anything that needs review
- warnings that should be reviewed before import

The dry run is read-only. It does not change Million Dollar Script 2 tables and does not create Million Dollar Script records.

Page detection is limited to legacy Million Dollar Script shortcodes, blocks, configured page options, and pages already associated with the selected migration source. Pages owned by the current Million Dollar Script installation and pages tied to a different legacy grid source are left unchanged.

### Import Million Dollar Script 2 Data And Deactivate Million Dollar Script 2

Choose this when you are ready to move to Million Dollar Script.

- Million Dollar Script imports supported Million Dollar Script 2 grids, blocks, unavailable/NFS blocks, packages, price zones, orders, placement media, grid background images and opacity, page mappings, and settings.
- Order inventory is reconciled with its linked legacy block rows so a stale saved block list does not silently omit paid inventory.
- Each legacy grid, order, block, placement, and page is connected through a migration identity rather than relying on matching database numbers.
- Original Million Dollar Script 2 page content is backed up in post meta before Million Dollar Script replaces the page embed.
- Million Dollar Script 2 source tables are not dropped or modified.
- After import completes, Million Dollar Script deactivates the old Million Dollar Script 2 plugin.

If an import is interrupted, run it again from the migration screen. A recovery run reconnects supported records and repairs missing migration relationships without creating duplicate orders or placements. Review the reconciliation details before deactivating Million Dollar Script 2; every supported source record should be imported once or listed with a reason it was skipped.

If deactivation cannot be completed, Million Dollar Script reports that clearly so you can handle it from Plugins.

### Deactivate Million Dollar Script 2 Without Importing

Choose this only if you want a clean Million Dollar Script start and do not want old Million Dollar Script 2 data imported.

- Million Dollar Script 2 is deactivated.
- Million Dollar Script 2 tables remain in the database.
- Million Dollar Script starts with the setup flow and can create a new first grid when you are ready.

## Shortcodes And Blocks

Million Dollar Script supports:

```text
[mds_grid id="1" read_only="false"]
[mds_grid id="1" read_only="true"]
[mds_grid id="1" renderer="classic"]
[mds3_page type="order" grid_id="1"]
```

When Million Dollar Script 2 is inactive, Million Dollar Script also handles legacy embeds:

```text
[milliondollarscript id="1" type="grid"]
[milliondollarscript id="1" type="order"]
[milliondollarscript type="list"]
```

Million Dollar Script also renders legacy block content such as Carbon Fields blocks named `carbon-fields/million-dollar-script`. When Million Dollar Script 2 is still active, Million Dollar Script intentionally leaves legacy shortcode and block ownership to Million Dollar Script 2 so both plugins can be reviewed side by side.

## Standard Pages

The setup screen can create missing standard pages:

- Pixel Grid
- Order Pixels
- Write Content
- Confirm Order
- Payment
- Manage Pixels
- Thank You
- Advertiser List
- Upload
- No Orders
- Statistics

Existing migrated pages are preserved. Million Dollar Script only creates missing page roles.

Pages that specify a legacy grid keep that grid relationship after import, including sites with several grids. A legacy page without an explicit grid uses a grid from the same migration source rather than an unrelated grid that may already exist in Million Dollar Script.

## After Import

1. Open Million Dollar Script > Grids and check the imported grid dimensions, block size, background presentation, packages, price zones, and unavailable regions.
2. Open the public grid page and verify the grid renders.
3. Test a small block selection and image upload.
4. Review checkout settings. WooCommerce sites should test checkout with a sandbox gateway. Standalone/manual sites should verify the Checkout URL and any Million Dollar Script 2 placeholders. See [Million Dollar Script Checkout And Payments](/docs/mds-3/million-dollar-script/3.0.0/main/mds3-checkout-and-payments).
5. Review Million Dollar Script > Orders for imported orders and new test orders.
6. Review the migration reconciliation totals. Investigate every skipped or warning entry before relying on the migrated site.
7. Keep the old Million Dollar Script 2 tables until you are confident the site is stable.

## Rollback Notes

Because Million Dollar Script does not drop or mutate Million Dollar Script 2 tables, rollback is normally:

1. Deactivate Million Dollar Script.
2. Reactivate Million Dollar Script 2.
3. Restore backed-up page content if you already imported and want the old page embeds back.

For a full rollback, restore the database and uploads backup taken before the upgrade.
