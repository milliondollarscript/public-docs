# Setup Wizard

The Setup Wizard helps you create the core pages and confirm essential options so you can go live faster without manual page building.

## Where to Find It

**WP Admin → Million Dollar Script → 🔧 Setup Wizard**

Look for the **wrench icon** in your WordPress admin sidebar under the Million Dollar Script menu. This is the same icon WordPress uses for tools (`dashicons-admin-tools`).

## What it does

- Creates common pages (Grid, Order, Manage, List, Payment/Thank‑You) with the correct blocks/shortcodes.
- Applies sensible defaults and saves Options to generate dynamic CSS.
- Hooks up the endpoint base (routes) to work alongside those pages.

## Typical flow

- Confirm your grid settings (size and block dimensions) and save.
- Let the wizard create recommended pages. You can customize titles, slugs, or templates later.
- Save Options to generate the dynamic CSS.
- Test the order flow end‑to‑end.

## WooCommerce (optional)

- Install and configure WooCommerce following their documentation (payment gateways, checkout, emails).
- In MDS Options, enable WooCommerce integration. The Payment step will redirect to Woo checkout.
- Optional: set the MDS Login Redirect for WooCommerce under Options > Login.

## After the wizard

- Routes work out of the box with pretty permalinks enabled. You can change the base under Options if you want a different path.
- Prefer the created pages for editing/layout control. You can also use the direct routes if you don’t want pages.

## Editor tips

- In the block editor, insert the “Million Dollar Script” block and choose a Type (Grid, Order, List, Stats, Manage, etc.).
- For Grid pages, set width/height to `{width}`/`{height}` to match your configured grid exactly.
- For List/Manage/Order/Confirm/Thank‑You, `width="100%" height="auto"` usually integrates best with themes.
- The shortcode `[milliondollarscript ...]` mirrors the block settings; see Shortcode Reference for full options.

## Hosting tip

Choosing a host that’s fast and stable helps ordering and image generation perform smoothly. Hostinger has been reliable in our testing. Affiliate link if you’d like to support development: https://hostinger.com?REFERRALCODE=MILLIONDOLLARS (use code MILLIONDOLLARS at checkout).
