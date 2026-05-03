# MDS3 Upgrade From MDS2

MDS3 is designed so existing MDS2 site owners can install it beside the old plugin, review the migration, and choose when to move. MDS3 does not automatically import MDS2 data or deactivate MDS2.

For a feature-by-feature view of what is covered and what still needs validation for a live site, see [MDS3 And MDS2 Workflow Parity](/docs/mds3-mds2-workflow-parity).

## Before You Start

- Back up the WordPress database and `wp-content/uploads`.
- Confirm the current MDS2 site works before changing anything.
- Keep the MDS2 plugin ZIP available in case you want to roll back.
- If WooCommerce is used for payments, confirm WooCommerce orders and checkout are healthy first.

## Install MDS3

1. Go to WordPress Admin > Plugins > Add New > Upload Plugin.
2. Upload the MDS3 ZIP.
3. Activate MDS3.
4. WordPress redirects you to Million Dollar Script > Setup.

If MDS2 is installed, MDS3 shows an "MDS2 Upgrade Choice" step. This step detects the old plugin package and the legacy `wp_mds_*` tables. No migration runs until you choose an action.

## Your Upgrade Choices

### Keep MDS2 Active For Now

Choose this when you want to compare MDS2 and MDS3 side by side.

- MDS2 remains active.
- MDS3 does not import old data.
- MDS3 does not take over the old `[milliondollarscript]` embeds while MDS2 is active.
- Use MDS3 pages and shortcodes for new MDS3 testing.

This is useful for review, but not recommended long term because both plugins manage similar admin menus, checkout concepts, and grid pages.

### Review Migration Dry Run

Open the migration dry run before importing. The dry run reports:

- MDS2 source table prefix
- MDS2 tables found and row counts
- page-wizard pages and shortcode/block pages
- MDS2 options that can be mapped
- target MDS3 tables
- warnings that should be reviewed before import

The dry run is read-only. It does not change MDS2 tables and does not create MDS3 records.

### Import MDS2 Data And Deactivate MDS2

Choose this when you are ready to move to MDS3.

- MDS3 imports supported MDS2 grids, blocks, unavailable/NFS blocks, packages, price zones, orders, media references, page mappings, and settings.
- Original MDS2 page content is backed up in post meta before MDS3 replaces the page embed.
- MDS2 source tables are not dropped or modified.
- After import completes, MDS3 deactivates the old MDS2 plugin.

If deactivation cannot be completed, MDS3 reports that clearly so you can handle it from Plugins.

### Deactivate MDS2 Without Importing

Choose this only if you want a clean MDS3 start and do not want old MDS2 data imported.

- MDS2 is deactivated.
- MDS2 tables remain in the database.
- MDS3 starts with the Million Dollar Script setup flow and can create a new first grid when you are ready.

## Shortcodes And Blocks

MDS3 supports:

```text
[mds_grid id="1" read_only="false"]
[mds_grid id="1" read_only="true"]
[mds_grid id="1" renderer="classic"]
[mds3_page type="order" grid_id="1"]
```

When MDS2 is inactive, MDS3 also handles legacy MDS2 embeds:

```text
[milliondollarscript id="1" type="grid"]
[milliondollarscript id="1" type="order"]
[milliondollarscript type="list"]
```

MDS3 also renders legacy MDS2 block content such as Carbon Fields blocks named `carbon-fields/million-dollar-script`. When MDS2 is still active, MDS3 intentionally leaves legacy MDS2 shortcode and block ownership to MDS2 so both plugins can be reviewed side by side.

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

Existing migrated pages are preserved. MDS3 only creates missing page roles.

## After Import

1. Open Million Dollar Script > Grids and check the imported grid dimensions, block size, packages, price zones, and unavailable regions.
2. Open the public grid page and verify the grid renders.
3. Test a small block selection and image upload.
4. Review checkout settings. WooCommerce sites should test checkout with a sandbox gateway. Standalone/manual sites should verify the Checkout URL and any MDS2 placeholders. See [MDS3 Checkout And Payments](/docs/mds3-checkout-and-payments).
5. Review Million Dollar Script > Orders for imported orders and new test orders.
6. Keep the old MDS2 tables until you are confident the site is stable.

## Rollback Notes

Because MDS3 does not drop or mutate MDS2 tables, rollback is normally:

1. Deactivate MDS3.
2. Reactivate MDS2.
3. Restore backed-up page content if you already imported and want the old page embeds back.

For a full rollback, restore the database and uploads backup taken before the upgrade.
