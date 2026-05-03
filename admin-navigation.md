# Admin Navigation

This is a quick map of the Million Dollar Script admin pages so you know where to look when configuring or troubleshooting.

## WordPress Sidebar

- **Million Dollar Script (Dashboard)**
  Landing page with the hero, internal dashboard menu, metrics, recent orders, system status, and service cards.

- **Extensions**
  The extension hub. This is the only operational submenu shown in the WordPress sidebar by default so users can quickly see that Million Dollar Script is extendable.

## Dashboard Menu

Most day-to-day pages are linked from the dashboard itself instead of being exposed as extra WordPress sidebar items.

- **Setup Wizard**
  First-run setup, standard pages, WooCommerce preference, and Million Dollar Script 2 upgrade choices.

- **Grids**
  Grid details, packages, price zones, unavailable regions, public page controls, and renderer settings.

- **Orders**
  Reservations, payment state, customer details, block assignments, and order status actions.

- **Settings**
  General, URLs, checkout, display, pixels, accounts, orders, and system settings.

- **Migration**
  Read-only migration dry run and import tools for Million Dollar Script 2 sites.

- **Documentation and Changelog**
  Links to current hosted docs and release notes.

## Extension Menu Items

Extensions should add their own internal dashboard links with the `mds3_dashboard_menu_items` filter. They can still register hidden admin pages, but they should avoid crowding the WordPress sidebar.

```php
add_filter( 'mds3_dashboard_menu_items', function ( array $groups ): array {
    $groups['extend']['items'][] = [
        'label' => __( 'Example Extension', 'example-extension' ),
        'url'   => admin_url( 'admin.php?page=example-extension' ),
        'icon'  => 'dashicons-admin-plugins',
    ];

    return $groups;
} );
```

For developers building extensions, see [Extension Development](/docs/extension-development).

## Notes

- Hidden pages remain directly accessible by URL and are linked from the dashboard menu.
- If you do not see an extension menu item, confirm the extension is active.
