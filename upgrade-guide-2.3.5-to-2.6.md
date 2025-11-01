# Upgrade Guide: 2.3.5 → 2.5/2.6

This guide highlights key changes when upgrading from older 2.3.5 sites to current 2.5/2.6.

## Before you start

- Back up your entire site, database and files.
- If you used a separate WordPress “bridge” or subdomain, plan to use the single plugin only.

## Core changes

- Single WordPress plugin embeds the MDS core—no separate integration plugin.
- Routes and pages: pretty endpoints and Gutenberg blocks/shortcodes; Setup Wizard can create pages for you.
- Database access via `$wpdb` for stability and performance.
- Dynamic CSS for theme modes and UI consistency.
- Optional WooCommerce integration for checkout/refunds.

## Steps

1) Update the plugin to the latest 2.5/2.6.
2) Visit WP Admin → Million Dollar Script → Setup Wizard to (re)create pages.
3) Open Options and save once to generate dynamic CSS.
4) If you changed routes/base, flush permalinks.
5) Verify ordering, upload, confirmation, and payment flows.
6) If enabling WooCommerce, install/configure it and then enable the MDS integration in Options.

## Pixel permalinks

- New configurable base and tokenized slug pattern; optional `mds-pixel` single pages.
- If you change the base/pattern, run the migration tool from Options to update slugs and preserve redirects.

## Known adjustments

- If custom templates/CSS targeted legacy markup, review under the `.mds-container` scope and adjust selectors.
- If you previously relied on global MDS body classes, note they’re now scoped; there is a legacy filter if you need temporary compatibility.

## Final checks

- Test multiple grids, selection alignment, and tooltip content.
- Confirm email templates, language, and currency settings.
- For WooCommerce: confirm checkout, order completion, and (optionally) refunds.

