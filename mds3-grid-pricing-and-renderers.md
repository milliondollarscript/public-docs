---
slug: mds3-grid-pricing-and-renderers
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [site-owners, administrators]
published: true
---

# Grid Pricing And Renderers

Million Dollar Script keeps the Million Dollar Script 2 grid-selling model, but the controls are now grouped under **Million Dollar Script > Grids > Edit Grid**.

## Base Pricing

Set **Price Per Block** on the grid details tab. This is the default unit price for any available block that is not covered by a price zone or selected package.

Set **Minimum Blocks Per Order** and **Maximum Blocks Per Order** on the same tab. Million Dollar Script enforces these limits in the frontend selection UI and again on the server when the order is reserved.

Set **Maximum Orders** to cap active orders for the grid. Active orders include reserved, pending-payment, and paid orders. Use `0` for no cap.

## Packages

Use the **Packages** tab to create Million Dollar Script 2-style package offers.

- **Package Price** replaces per-block pricing for the selected package.
- **Duration Days** is stored with the order and shown in order metadata.
- **Max Orders** caps active orders that use that package.
- **Default Package** preselects the package on the public grid.
- Archived packages remain in historical order data but are no longer offered.

Customers see package choices beside the reserve button. The selection summary updates with the estimated total before reservation. Million Dollar Script still validates the selected package on the server, so stale or tampered package IDs are rejected.

## Price Zones

Use the **Price Zones** tab to define regional per-block prices.

You can define a zone by row/column bounds or by legacy block ID bounds. Migrated Million Dollar Script 2 price rows are imported here. The editor previews active zones over the grid, and the public grid shows a light zone overlay while customers choose blocks.

When multiple zones match a block, Million Dollar Script uses the most specific match. Block-level price overrides win over price zones, and package prices win over both.

## Unavailable Regions

Use the **Availability** tab to mark regions unavailable or available. This replaces old NFS/unavailable block management without materializing every block in very large grids.

Sold and reserved blocks are protected when a region is changed.

## Renderer Modes

Million Dollar Script supports three renderer values:

- `auto` - default. Uses OpenLayers when available and falls back to the classic canvas renderer.
- `openlayers` - preferred interactive renderer for large or zoomable grids.
- `classic` - canvas fallback for themes or environments where OpenLayers is not desired.

Set the grid default in **Grids > Edit Grid > Details**, or override a page embed:

```text
[mds_grid id="1" read_only="false" renderer="classic"]
```

Both renderer paths share the same selection, pricing, package, tooltip, image-placement, and checkout logic.
