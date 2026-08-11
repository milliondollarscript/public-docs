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

## Add pages to your site navigation

Creating or mapping a page does not automatically add it to your theme's navigation. This protects existing menus and lets you choose which customer-facing pages to publish. Start in **Pages > All Pages** to confirm that the pages you want are published, then use the workflow for your active theme.

### Classic themes

1. Open **Appearance > Menus**.
2. Select the menu and display location used by your site header, or create a menu if needed.
3. Under **Pages**, choose the published Million Dollar Script pages and select **Add to Menu**.
4. Arrange the links, adjust their navigation labels if needed, and select **Save Menu**.

WordPress provides the complete [Appearance Menus Screen guide](https://wordpress.org/documentation/article/appearance-menus-screen/), including menu locations, ordering, and submenus. If **Appearance > Menus** is unavailable, your theme may use the Site Editor instead.

### Block themes

1. Open **Appearance > Editor**. The Site Editor is available when a block theme is active.
2. Open **Navigation**, or edit the header or other template part that contains your site's Navigation block.
3. Select the Navigation block, add a menu item, and search for the published Million Dollar Script page. You can also enter its URL as a custom link.
4. Arrange the links and save the Navigation and template changes.

See WordPress's [Site Editor guide](https://wordpress.org/documentation/article/site-editor/) and [Navigation block guide](https://wordpress.org/documentation/article/navigation-block/) for the current editor controls, menu selection, submenus, and responsive display options.

### Pages added by extensions

Some extensions recommend or create additional pages after activation. Complete the extension's setup action first, confirm the page under **Pages > All Pages**, and then add it to the same classic menu or Navigation block. Removing a navigation item does not delete its WordPress page, and deactivating an extension does not automatically remove links you chose to publish.

For sites with several grids, use clear labels such as **Order Main Grid** or group supporting pages in a submenu. Test the final navigation on a narrow screen and as a logged-out visitor before launch.

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
