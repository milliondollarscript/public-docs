# Admin Navigation

This is a quick map of the Million Dollar Script admin pages so you know where to look when configuring or troubleshooting.

## Top-level

- **Million Dollar Script (Dashboard)**  
  Landing page with links to key areas and version info.

## Key submenus

- **Admin**  
  Legacy core administration (Manage Grids, Price Zones, Not For Sale, Backgrounds, Orders lists, Click Reports, Process Pixels, Main Config, System Information, License, etc.).  
  Use this when you need fine‑grained grid controls or to view operational reports.

- **Options**  
  Carbon Fields options for routes, theme, pixels, login, WooCommerce, logging, and more.  
  Saving Options regenerates dynamic CSS.

- **Setup Wizard**  
  Creates recommended pages (Grid, Order, Manage, List, Payment/Thank‑You), applies defaults, and saves Options.

- **Logs**  
  Toggle logging, clear the log, and view live updates. Useful during setup and payment flow testing.

- **Emails**  
  Configure email settings/messages used by the plugin (where applicable).

- **Changelog**  
  View recent feature and fix summaries bundled with the plugin.

- **Extensions**  
  Curated launch point for extension UIs. The core plugin hides all other legacy submenus by default and highlights this entry.  
  Extensions add their own links by hooking into `mds_main_menu_extensions_submenu`. Example:
  ```php
  add_action( 'mds_main_menu_extensions_submenu', function () {
      if ( current_user_can( 'manage_options' ) ) {
          echo '<li><a href="' . esc_url( admin_url( 'admin.php?page=mds-example' ) ) . '">Example Extension</a></li>';
      }
  } );
  ```

- **Approve Pixels (utility)**  
  Dedicated screen for reviewing and bulk‑approving orders/pixels.

## Hidden legacy submenus & filters

- Hidden entries are tagged with the CSS class `mds-hidden-submenu`, preventing the “flash” on load while keeping the pages directly accessible via URL.  
- To expose additional Million Dollar Script submenus, use the filter:
  ```php
  add_filter( 'mds_extensions_visible_submenus', function ( array $slugs ) {
      $slugs[] = 'mds-example';
      $slugs[] = 'mds-example2';
      return $slugs;
  } );
  ```
  Any slug you add is shown immediately without modifying the core plugin.

## Notes

- Some screens are modern wrappers around the legacy MDS core; others are fully WordPress‑native.
- If you don’t see a submenu referenced here, it may be hidden until certain options are saved or features are enabled (e.g., WooCommerce).
