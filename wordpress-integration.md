# WordPress Integration

The integration is no longer a separate plugin or service - Million Dollar Script Two is a single WordPress plugin that embeds the full MDS core and native WP features.

## What you get

- Theme-native pages and routes with your header/footer and templates
- Responsive grid rendering and UI
- A Gutenberg block and a flexible shortcode (`[milliondollarscript ...]`)
- Pretty endpoints (default base: `milliondollarscript`) for grid/order/manage/list/payment/thank‑you
- Optional WooCommerce: checkout, refunds, account/register/login coordination
- Options stored in WordPress; dynamic CSS for light/dark, accessible colors, and button styles

## Do I still need a subdomain or a second database?

No. The legacy approach (separate subdomain with a bridge) is replaced. The plugin ships the MDS core inside WordPress, manages database upgrades via `$wpdb`, and integrates with WP post types (e.g., `mds-pixel`).

## Blocks vs. shortcodes vs. routes

- Blocks: easiest way to add MDS views to pages in the editor.
- Shortcode: `[milliondollarscript id="1" type="grid|users|list|stats|manage|payment|thank-you" width="100%" height="auto"]`
- Routes: visit `/milliondollarscript/order`, `/milliondollarscript/manage`, `/milliondollarscript/list`, etc. Change the base in Options.

Use whichever fits your theme and navigation. Routes require no extra pages; blocks/shortcodes let you compose within your site’s structure.

## WooCommerce notes

- Enable under Options > WooCommerce. When enabled, the Payment step redirects to the WooCommerce checkout.
- Refund hooks are supported. Login/register can be coordinated with Woo’s My Account when the option is enabled.

## Migration notes (legacy installs)

- If you’re coming from an older 2.x install with a separate WP bridge, use the current plugin directly. The `mds-pixel` permalink base and slug pattern are configurable; a migration tool helps update existing slugs and preserves redirects.
- After enabling the plugin, flush permalinks once if you see 404s on `/milliondollarscript/...` routes.

