# Known Compatibility Notes

This supersedes the old compatibility note. Here are the current, practical considerations.

## Hosting & Platform

- WordPress.org required. Managed “wordpress.com” sites that don’t allow custom plugins/themes are not supported.
- PHP 8.1+ and WordPress 6.7+ are required.
- Image processing can be memory‑intensive for large grids; allocate a sensible PHP memory limit per your host’s guidance.
- Practical pick: Hostinger has been a reliable option for MDS sites (fast, stable, easy WordPress setup). If you’d like to support development, you can use the affiliate link: https://hostinger.com?REFERRALCODE=MILLIONDOLLARS (referral).

## Permalinks & Routes

- Pretty permalinks must be enabled for `/milliondollarscript/...` routes (base is configurable in Options).
- If routes 404 after activation or after changing the base, flush permalinks once.

## WooCommerce

- When enabled in Options, the Payment step redirects to the WooCommerce checkout. The plugin suppresses Woo’s “empty checkout” redirect on MDS routes so the flow is reliable regardless of the checkout page slug.
- Refund hooks and login/register coordination are supported. Keep WooCommerce up to date for best compatibility.

## Theme/Plugin Interactions

- Page builders and custom themes generally work. If a builder template strips the content container or overly constrains image sizes, selections can appear offset—see [Troubleshooting: Grid Alignment](troubleshooting.md#grid-alignment).
- Elementor: earlier conflicts were addressed; upgrade to the latest MDS version if you experienced activation conflicts in the past.

## Multisite

- Multisite is supported when the plugin is network‑enabled per site’s needs. Ensure each site meets PHP/WP minimums.

If you run into an environment‑specific limitation, capture details (WP/PHP versions, active theme/plugins, hosting type) and reproduce on a default theme to isolate the cause.

For step-by-step solutions to common problems, see the [Troubleshooting Guide](troubleshooting.md).
