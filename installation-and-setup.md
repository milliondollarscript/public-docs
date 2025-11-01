# Million Dollar Script: Installation & Setup

This guide updates and replaces the old “WordPress Installation” article. The core WordPress integration is now built into a single plugin: Million Dollar Script Two. No separate WP integration plugin is required.

## Requirements

- WordPress 6.7+ (tested up to 6.8)
- PHP 8.1+
- Optional: WooCommerce (for checkout, refunds, and account integration)

## Hosting Recommendation

If you’re choosing hosting for a new MDS site, I’ve had consistently good results with Hostinger for performance, stability, and ease of setup. Their stack plays nicely with our grid/image generation and keeps ordering flows snappy.

- Fast page loads and solid uptime
- Free SSL and automated backups
- One‑click WordPress setup and helpful support

Use my affiliate link if you’d like to support development:

- Hostinger: https://hostinger.com?REFERRALCODE=MILLIONDOLLARS (referral)
- Tip: apply referral code MILLIONDOLLARS at checkout for an extra discount

## Install & Activate

- Install the plugin into `wp-content/plugins/milliondollarscript-two` and activate it from Plugins.
- After activation, the plugin registers routes, options, cron, and admin pages. If you see 404s on routes, flush permalinks once in Settings > Permalinks (save), or via WP‑CLI: `wp rewrite flush --hard`.

## First Steps

Use the Setup Wizard to get the basics in place, then decide whether you prefer pretty routes or page-based embeds.

### Run the Setup Wizard

- Go to Million Dollar Script > Setup Wizard in WP Admin.
- Let the wizard create the standard pages (Grid, Order, Manage, List, etc.).
- Save Options at the end so dynamic CSS is generated and settings persist.

You can adjust or delete any created page later and use blocks/shortcodes as needed.

- Pretty routes (no pages): visit `/milliondollarscript/order`, `/milliondollarscript/manage`, `/milliondollarscript/list`, `/milliondollarscript/payment`, `/milliondollarscript/thank-you`, etc. The base segment is configurable in Options.
- Page-based flow (optional): add the MDS block or shortcode to your pages. This lets you use your theme’s templates, builders, and menus.

### Recommended Pages (optional)

Create pages and insert the block “Million Dollar Script”, or use the shortcode `[milliondollarscript ...]`.

- Grid
  - Block type: Grid
  - Shortcode: `[milliondollarscript id="1" type="grid" width="{width}" height="{height}" align="center"]`
  - Notes: `{width}` and `{height}` automatically resolve to your grid’s exact dimensions. If you prefer, you can specify explicit pixel sizes (e.g., `1000px`).

- Buy Pixels / Order
  - Block type: Order Pixels (or Write Your Ad, then Confirm Order)
  - Shortcode: `[milliondollarscript id="1" type="users" width="100%" height="auto"]`
  - Notes: `type="users"` is supported for legacy parity; `type="order"` and `type="write-ad"` split the flow into steps if you prefer that layout.

- List (Advertisers)
  - Block type: Ads List
  - Shortcode: `[milliondollarscript type="list" width="100%" height="auto"]`
  - Notes: When no `id` is provided, the list shows all grids.

- Stats Box (optional)
  - Block type: Stats box
  - Shortcode: `[milliondollarscript id="1" type="stats" width="150px" height="60px"]`

- Manage Pixels (optional public page)
  - Block type: Manage Pixels
  - Shortcode: `[milliondollarscript id="1" type="manage" width="100%" height="auto"]`

- Payment and Thank‑You (optional as pages)
  - Block types: Payment, Thank‑You
  - Shortcodes:
    - `[milliondollarscript type="payment"]`
    - `[milliondollarscript type="thank-you"]`
  - Notes: When WooCommerce is enabled, the Payment step redirects to the Woo checkout.

## Options & Styling

- Options live under “Million Dollar Script > Options”.
- Theme and color settings generate a dynamic CSS file stored under uploads. Saving Options regenerates it. You can also regenerate via WP‑CLI if needed:
  - `wp eval '\\MillionDollarScript\\Classes\\Web\\Styles::save_dynamic_css_file(); echo "OK\n";'`
- Pixel permalink base and slug pattern are configurable (see Options > Pixels). The `mds-pixel` post type pages are optionally enabled.

## WooCommerce Integration (optional)

- Enable under Options > WooCommerce. When enabled, the Payment step redirects to Woo checkout; refunds and account pages integrate cleanly.
- There’s an optional login redirect (Options > Login) to control where users land after WooCommerce login.

## Notes & Tips

- Grid dimensions: define grid width/height and block size under MDS Admin (Manage Grids). If you use the block, `{width}`/`{height}` automatically match your grid. With the shortcode, `Functions::maybe_set_dimensions()` can derive dimensions as needed, but for the Grid view prefer the placeholders above.
- Multiple grids per page are supported. The UI and tooltips are scoped so clicks and popups map to the correct grid.
- If you switch your endpoint base (Options > Routes), flush permalinks.
- If block selections don’t align with the image, see “Troubleshooting: Grid Alignment”.
