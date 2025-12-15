# Routes & Pages

MDS supports “routes” (pretty URLs) and page-based embeds (blocks/shortcodes). Use either or both.

## Routes (pretty URLs)

- Default base: `/milliondollarscript` (configurable in Options).
- Examples:
  - `/milliondollarscript/order`
  - `/milliondollarscript/manage`
  - `/milliondollarscript/list`
  - `/milliondollarscript/payment`
  - `/milliondollarscript/thank-you`
- Requirements: Pretty permalinks enabled in WordPress.
- Troubleshooting: If a route 404s, see [Troubleshooting: Routes & 404 Errors](troubleshooting.md#routes--404-errors).

## Pages (blocks or shortcode)

- You can change the permalinks of these pages to whatever you want on your site.
- Use the Setup Wizard to create standard WordPress pages with the correct blocks.
- You can also manually insert the “Million Dollar Script” block or the `[milliondollarscript]` shortcode into any page.
- Blocks let you use `{width}`/`{height}` placeholders for the Grid to match exact dimensions automatically.

## Dynamic Page Container (advanced)

- The plugin can render routes inside a designated WP page so your theme’s template applies (header/footer). This uses the “dynamic page” option internally.
- Most sites can ignore this—use the created pages and/or routes as needed.

## Changing the base

- Update the endpoint base in Options.
- After changing it, flush permalinks once. This should be automatically done by the plugin, but you can also do it manually in WordPress Settings > Permalinks.

## Upgrading from legacy 2.3.5

- Old installs sometimes used a separate integration or different paths.
- After upgrading to 2.5/2.6, use the Setup Wizard to (re)create pages and confirm Options.
- If you previously hardcoded paths, update links to the new routes or to your created pages.

