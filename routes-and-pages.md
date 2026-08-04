---
slug: routes-and-pages
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [site-owners, administrators]
published: true
---

# Routes & Pages

Million Dollar Script supports “routes” (pretty URLs) and page-based embeds (blocks/shortcodes). Use either or both.

## Routes (pretty URLs)

- Default base: `/milliondollarscript` (configurable in Options).
- Examples:
  - `/milliondollarscript/order`
  - `/milliondollarscript/manage`
  - `/milliondollarscript/list`
  - `/milliondollarscript/payment`
  - `/milliondollarscript/thank-you`
- Requirements: Pretty permalinks enabled in WordPress.
- Troubleshooting: If a route 404s, see [Troubleshooting: Routes & 404 Errors](/docs/mds-3/million-dollar-script/3.0.0/main/troubleshooting#routes--404-errors).

## Pages (blocks or shortcode)

- You can change the permalinks of these pages to whatever you want on your site.
- Use the Setup Wizard to create standard WordPress pages with the correct blocks.
- You can also manually insert the **Grid Embed** block, the **Stats Widget** block, or shortcodes into any page.
- The Grid block lets you use `{width}` and `{height}` placeholders to match the selected grid’s exact dimensions automatically.
- Statistics can be embedded with the Stats block or `[mds3_page type="stats" grid_id="1"]`; they do not need a dedicated setup page.

## Dynamic Page Container (advanced)

- The plugin can render routes inside a designated WP page so your theme’s template applies (header/footer). This uses the “dynamic page” option internally.
- Most sites can ignore this. Use the created pages and/or routes as needed.

## Changing the base

- Update the endpoint base in Options.
- After changing it, flush permalinks once. This should be automatically done by the plugin, but you can also do it manually in WordPress Settings > Permalinks.

## Upgrading from legacy 2.3.5

- Old installs sometimes used a separate integration or different paths.
- After upgrading to 2.5/2.6, use the Setup Wizard to (re)create pages and confirm Options.
- If you previously hardcoded paths, update links to the new routes or to your created pages.
