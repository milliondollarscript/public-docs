---
slug: setup-wizard
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [site-owners, administrators]
published: true
---

# Setup Wizard

The Setup Wizard helps you create the core pages and confirm essential options so you can go live faster without manual page building.

If you want to see the finished flow before configuring your own site, [try the browser demo](/demo). It opens a private, disposable WordPress workspace with synthetic campaign data and guided missions.

## Where to Find It

Open **WP Admin > Million Dollar Script > Setup**. The page heading is **Setup Wizard**.

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
- If you are building a new site, optionally select **Create the optional starter site when setup is saved**. This adds Blog, Contact, and About pages and prepares navigation for Home, Blog, Order Pixels, Manage Pixels, Contact, and About.
- Save Options to generate the dynamic CSS.
- Test the order flow end‑to‑end.

## Optional starter site

The starter-site option is unchecked by default and becomes available after the standard Million Dollar Script pages are ready. It is intended for a new site that needs a useful editable starting structure.

The workflow preserves an existing static front page. When the site does not already use a static front page, it assigns the grid page as Home. It reuses matching published Blog and About pages, creates only the missing pages, and never replaces their content on later setup saves.

The Contact page uses a Million Dollar Script wrapper rather than an extension shortcode that could appear broken. Until the free Contact Form extension is active, visitors see a short unavailable message and administrators see an activation link. Activating Contact Form makes the same page render the form automatically.

For a new block-theme site without saved Navigation, the workflow creates an editable Navigation containing the six recommended links. For a classic theme, it creates an editable menu and assigns it only when a suitable menu location is unoccupied. Existing Navigation, menus, location assignments, and front-page choices are preserved. Setup labels the navigation for review when manual placement is needed; use the steps in [Routes & Pages](/docs/mds-3/million-dollar-script/3.0.0/main/routes-and-pages#add-pages-to-your-site-navigation) to add the new pages to an existing header.

## Payment Provider

- Use **Standalone/manual checkout** for offline payment or custom Checkout URL flows.
- Install a payment provider extension when a shop or gateway should process payments.
- For WooCommerce, install and configure WooCommerce, then install **Million Dollar Script WooCommerce Checkout** and choose **WooCommerce** in Setup.
- Optional: when the WooCommerce checkout extension is active, set the WooCommerce Login Redirect under **Million Dollar Script → Settings → WooCommerce Checkout**.

## After the wizard

- Routes work out of the box with pretty permalinks enabled. You can change the base under Options if you want a different path.
- Prefer the created pages for editing/layout control. You can also use the direct routes if you don’t want pages.
- If you did not use the optional starter site, or it preserved an existing menu, add the customer-facing pages you want to publish to your classic menu or block-theme Navigation. See [Routes & Pages](/docs/mds-3/million-dollar-script/3.0.0/main/routes-and-pages#add-pages-to-your-site-navigation) for both WordPress workflows.

## Editor tips

- In the block editor, insert **Grid Embed** for grid pages.
- Insert **Stats Widget** anywhere you want to show sold and available totals. It can display blocks or pixels, depending on the block setting or the plugin default.
- Use `[mds_grid id="1" read_only="false"]` for an interactive grid, replacing `1` with the grid id you want to show.
- Use `[mds3_page type="manage" grid_id="1"]`, `[mds3_page type="list" grid_id="1"]`, and the other standard page types for supporting pages, replacing `1` per page when you run multiple grids.
- Legacy `[milliondollarscript ...]` embeds are supported after Million Dollar Script 2 is inactive; see Shortcode Reference for full options.

## Hosting tip

Choosing a host that’s fast and stable helps ordering and image generation perform smoothly. Hostinger has been reliable in our testing. Affiliate link if you’d like to support development: https://hostinger.com?REFERRALCODE=MILLIONDOLLARS (use code MILLIONDOLLARS at checkout).
