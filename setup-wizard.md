# Setup Wizard

The Setup Wizard helps you create the core pages and confirm essential options so you can go live faster without manual page building.

## Where to Find It

**WP Admin → Million Dollar Script → 🔧 Setup Wizard**

Look for the **wrench icon** in your WordPress admin sidebar under the Million Dollar Script menu.

## What it does

- Detects an installed MDS2 plugin and asks whether to keep it active, review migration, import data, or deactivate it.
- Creates common pages (Grid, Order, Manage, List, Payment/Thank‑You) with the correct blocks/shortcodes.
- Applies sensible defaults and saves Options to generate dynamic CSS.
- Hooks up the endpoint base (routes) to work alongside those pages.

## Typical flow

- Confirm your grid settings (size and block dimensions) and save.
- If MDS2 is detected, choose whether to keep it active for now or review/import migration. MDS3 will not migrate or deactivate MDS2 automatically.
- Let the wizard create recommended pages. You can customize titles, slugs, or templates later.
- Save Options to generate the dynamic CSS.
- Test the order flow end‑to‑end.

## WooCommerce (optional)

- Install and configure WooCommerce following their documentation (payment gateways, checkout, emails).
- In MDS Options, enable WooCommerce integration when WooCommerce should process payment. MDS3 sends customers to WooCommerce checkout after artwork upload.
- Optional: set the MDS Login Redirect for WooCommerce under Options > Login.

## After the wizard

- Routes work out of the box with pretty permalinks enabled. You can change the base under Options if you want a different path.
- Prefer the created pages for editing/layout control. You can also use the direct routes if you don’t want pages.

## Editor tips

- In the block editor, insert the “MDS Grid” block for grid pages.
- Use `[mds_grid id="1" read_only="false"]` for an interactive grid, replacing `1` with the grid id you want to show.
- Use `[mds3_page type="manage" grid_id="1"]`, `[mds3_page type="list" grid_id="1"]`, and the other standard page types for supporting pages, replacing `1` per page when you run multiple grids.
- Legacy `[milliondollarscript ...]` embeds are supported after MDS2 is inactive; see Shortcode Reference for full options.

## Hosting tip

Choosing a host that’s fast and stable helps ordering and image generation perform smoothly. Hostinger has been reliable in our testing. Affiliate link if you’d like to support development: https://hostinger.com?REFERRALCODE=MILLIONDOLLARS (use code MILLIONDOLLARS at checkout).
