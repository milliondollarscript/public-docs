---
slug: mds3-mds2-workflow-parity
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [site-owners, administrators]
published: true
---

# Million Dollar Script 3.0 And Million Dollar Script 2 Workflow Parity

Million Dollar Script 3.0 is a clean rewrite, so it does not copy every Million Dollar Script 2 screen one-for-one. The upgrade goal is that site owners can keep selling ads, preserve existing grids and orders, and understand which old workflows now use a newer Million Dollar Script path.

## Covered Upgrade Workflows

These workflows are supported by Million Dollar Script migration or compatibility handling:

- Install Million Dollar Script 3.0 beside Million Dollar Script 2 without automatically importing data.
- Keep Million Dollar Script 2 active while testing Million Dollar Script 3.0 pages and grids.
- Import Million Dollar Script 2 grids, dimensions, block sizes, packages, price zones, orders, order items, placements, unavailable/NFS blocks, page assignments, and mapped settings.
- Preserve original Million Dollar Script 2 page content in post meta before replacing the page embed.
- Keep Million Dollar Script 2 source tables in place; Million Dollar Script 3.0 does not drop them.
- Render migrated grid pages through Million Dollar Script 3.0.
- Render legacy Million Dollar Script 2 shortcodes and Carbon Fields blocks through Million Dollar Script 3.0 when Million Dollar Script 2 is inactive.
- Leave legacy shortcode and block ownership to Million Dollar Script 2 when Million Dollar Script 2 is still active.
- Display active advertiser placements on migrated advertiser-list pages.
- Let signed-in customers review their Million Dollar Script orders from migrated manage pages.
- Let customers use private order-key links to upload or update artwork.
- Preserve Million Dollar Script 2-style unavailable/NFS blocks as unavailable inventory.
- Preserve price-zone and package pricing for new reservations, including package/order caps and frontend estimated totals.
- Sync paid provider orders back to Million Dollar Script order and block status when a payment provider extension is active.
- Route standalone/manual checkout through the configured Checkout URL, including Million Dollar Script 2 placeholder replacement.
- Send no-URL manual checkout flows to the thank-you/order-summary page after upload.

## Changed Workflows

Million Dollar Script 3.0 uses a grid-first customer flow. Instead of requiring separate Million Dollar Script 2 pages for every step, customers usually select blocks, reserve, upload artwork, and continue to payment from the grid page.

Legacy pages such as Order Pixels, Confirm Order, Payment, Upload, Manage Pixels, Thank You, Advertiser List, No Orders, and Statistics are still created or migrated so old navigation and content do not go blank. Some of those pages now guide customers back to the grid or show order-specific content when opened with a Million Dollar Script order key.

## Remaining Validation Areas

Do not treat a Million Dollar Script migration as fully production-proven until these areas have been reviewed for the target site:

- Real Million Dollar Script 2 schema variants, especially installs without a legacy `config` table.
- Any custom Million Dollar Script 2 payment extensions or direct gateway integrations that are not WooCommerce or the core standalone/manual Checkout URL flow.
- Gateway webhook reliability for custom payment extensions.
- Renderer behavior on the target theme, especially if a site forces `renderer="classic"` or has aggressive script optimization.
- Large-grid performance and selection accuracy on the target theme.
- Any custom Million Dollar Script 2 templates, custom extensions, or direct database integrations.

## Recommended Acceptance Test

Before switching a live site fully to Million Dollar Script 3.0:

1. Back up the database and `wp-content/uploads`.
2. Activate Million Dollar Script 3.0 and keep Million Dollar Script 2 active until the migration dry run has been reviewed.
3. Run the Million Dollar Script 3.0 migration dry run and confirm counts for grids, orders, blocks, packages, price zones, pages, and placements.
4. Import into Million Dollar Script 3.0 only after counts look right.
5. Open the migrated grid, advertiser list, manage, upload, payment, thank-you, and stats pages.
6. Reserve new blocks and confirm unavailable/NFS blocks cannot be selected.
7. Upload artwork, set a destination URL, and verify the ad appears on the grid.
8. If WooCommerce is enabled, complete a sandbox checkout and confirm the Million Dollar Script order becomes paid and the selected blocks become sold.
9. Keep the old Million Dollar Script 2 plugin ZIP and database tables until the migrated site has been reviewed.
