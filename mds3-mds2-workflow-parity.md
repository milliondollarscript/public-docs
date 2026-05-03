# MDS3 And MDS2 Workflow Parity

MDS3 is a clean rewrite, so it does not copy every MDS2 screen one-for-one. The upgrade goal is that site owners can keep selling ads, preserve existing grids and orders, and understand which old workflows now use a newer MDS3 path.

## Covered Upgrade Workflows

These workflows are supported by MDS3 migration or compatibility handling:

- Install MDS3 beside MDS2 without automatically importing data.
- Keep MDS2 active while testing MDS3 pages and grids.
- Import MDS2 grids, dimensions, block sizes, packages, price zones, orders, order items, placements, unavailable/NFS blocks, page assignments, and mapped settings.
- Preserve original MDS2 page content in post meta before replacing the page embed.
- Keep MDS2 source tables in place; MDS3 does not drop them.
- Render migrated grid pages through MDS3.
- Render legacy MDS2 shortcodes and Carbon Fields blocks through MDS3 when MDS2 is inactive.
- Leave legacy shortcode and block ownership to MDS2 when MDS2 is still active.
- Display active advertiser placements on migrated advertiser-list pages.
- Let signed-in customers review their MDS3 orders from migrated manage pages.
- Let customers use private order-key links to upload or update artwork.
- Preserve MDS2-style unavailable/NFS blocks as unavailable inventory.
- Preserve price-zone and package pricing for new reservations, including package/order caps and frontend estimated totals.
- Sync paid WooCommerce orders back to MDS3 order and block status when WooCommerce integration is enabled.
- Route standalone/manual checkout through the configured Checkout URL, including MDS2 placeholder replacement.
- Send no-URL manual checkout flows to the thank-you/order-summary page after upload.

## Changed Workflows

MDS3 uses a grid-first customer flow. Instead of requiring separate MDS2 pages for every step, customers usually select blocks, reserve, upload artwork, and continue to payment from the grid page.

Legacy pages such as Order Pixels, Confirm Order, Payment, Upload, Manage Pixels, Thank You, Advertiser List, No Orders, and Statistics are still created or migrated so old navigation and content do not go blank. Some of those pages now guide customers back to the grid or show order-specific content when opened with an MDS3 order key.

## Remaining Validation Areas

Do not treat an MDS3 migration as fully production-proven until these areas have been reviewed for the target site:

- Real MDS2 schema variants, especially installs without a legacy `config` table.
- Any custom MDS2 payment extensions or direct gateway integrations that are not WooCommerce or the core standalone/manual Checkout URL flow.
- Gateway webhook reliability for custom payment extensions.
- Renderer behavior on the target theme, especially if a site forces `renderer="classic"` or has aggressive script optimization.
- Large-grid performance and selection accuracy on the target theme.
- Any custom MDS2 templates, custom extensions, or direct database integrations.

## Recommended Acceptance Test

Before switching a live site fully to MDS3:

1. Back up the database and `wp-content/uploads`.
2. Activate MDS3 and keep MDS2 active until the migration dry run has been reviewed.
3. Run the MDS3 migration dry run and confirm counts for grids, orders, blocks, packages, price zones, pages, and placements.
4. Import into MDS3 only after counts look right.
5. Open the migrated grid, advertiser list, manage, upload, payment, thank-you, and stats pages.
6. Reserve new blocks and confirm unavailable/NFS blocks cannot be selected.
7. Upload artwork, set a destination URL, and verify the ad appears on the grid.
8. If WooCommerce is enabled, complete a sandbox checkout and confirm the MDS3 order becomes paid and the selected blocks become sold.
9. Keep the old MDS2 plugin ZIP and database tables until the migrated site has been reviewed.
