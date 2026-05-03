# Setup Wizard

The Setup Wizard helps you create the core pages and confirm essential options so you can go live faster without manual page building.

## Where to Find It

**WP Admin → Million Dollar Script → 🔧 Setup Wizard**

Look for the **wrench icon** in your WordPress admin sidebar under the Million Dollar Script menu.

## What it does

- Detects an installed Million Dollar Script 2 plugin and asks whether to keep it active, review migration, import data, or deactivate it.
- Lets you choose the active payment provider. Provider extensions can add WooCommerce, EDD, or other checkout systems.
- Creates common pages (Grid, Order, Manage, List, Payment/Thank‑You) with the correct blocks/shortcodes. Statistics are available as a block or shortcode but are not required as a setup page.
- Applies sensible defaults and saves Options to generate dynamic CSS.
- Hooks up the endpoint base (routes) to work alongside those pages.

## Typical flow

- Confirm your grid settings (size and block dimensions) and save.
- If Million Dollar Script 2 is detected, choose whether to keep it active for now or review/import migration. Million Dollar Script will not migrate or deactivate Million Dollar Script 2 automatically.
- Let the wizard create recommended pages. You can customize titles, slugs, or templates later.
- Save Options to generate the dynamic CSS.
- Test the order flow end‑to‑end.

## Payment Provider

- Use **Standalone/manual checkout** for offline payment or custom Checkout URL flows.
- Install a payment provider extension when a shop or gateway should process payments.
- For WooCommerce, install and configure WooCommerce, then install **Million Dollar Script WooCommerce Checkout** and choose **WooCommerce** in Setup.
- Optional: when the WooCommerce checkout extension is active, set the WooCommerce Login Redirect under **Million Dollar Script → Settings → WooCommerce Checkout**.

## After the wizard

- Routes work out of the box with pretty permalinks enabled. You can change the base under Options if you want a different path.
- Prefer the created pages for editing/layout control. You can also use the direct routes if you don’t want pages.

## Editor tips

- In the block editor, insert the “Million Dollar Script Grid” block for grid pages.
- Insert the “Million Dollar Script Stats” block anywhere you want to show sold and available totals. It can display blocks or pixels, depending on the block setting or the plugin default.
- Use `[mds_grid id="1" read_only="false"]` for an interactive grid, replacing `1` with the grid id you want to show.
- Use `[mds3_page type="manage" grid_id="1"]`, `[mds3_page type="list" grid_id="1"]`, and the other standard page types for supporting pages, replacing `1` per page when you run multiple grids.
- Legacy `[milliondollarscript ...]` embeds are supported after Million Dollar Script 2 is inactive; see Shortcode Reference for full options.

## Hosting tip

Choosing a host that’s fast and stable helps ordering and image generation perform smoothly. Hostinger has been reliable in our testing. Affiliate link if you’d like to support development: https://hostinger.com?REFERRALCODE=MILLIONDOLLARS (use code MILLIONDOLLARS at checkout).
