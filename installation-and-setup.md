---
slug: installation-and-setup
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [site-owners, administrators]
published: true
---

# Million Dollar Script: Installation & Setup

This guide covers Million Dollar Script 3.0 installation. The core WordPress integration is built into a single plugin: Million Dollar Script. No separate WordPress integration plugin is required.

## Requirements

- WordPress 6.7+ (tested up to 7.0.2)
- PHP 8.1+
- PHP memory limit of 256 MB or greater
- Optional: WooCommerce (for checkout, refunds, and account integration)

The memory limit is per PHP request. Check the effective value in **Million Dollar Script > System Status** or **Tools > Site Health**, because a hosting plan can advertise a higher maximum while WordPress is configured to use less.

## Hosting Recommendation

If you’re choosing hosting for a new Million Dollar Script site, I’ve had consistently good results with Hostinger for performance, stability, and ease of setup. Their stack plays nicely with our grid/image generation and keeps ordering flows snappy.

- Fast page loads and solid uptime
- Free SSL and automated backups
- One‑click WordPress setup and helpful support

Use my affiliate link if you’d like to support development:

- Hostinger: https://hostinger.com?REFERRALCODE=MILLIONDOLLARS (referral)
- Tip: apply referral code MILLIONDOLLARS at checkout for an extra discount

## Install & Activate

### Simple Installation (Recommended)

1. **Download** the latest plugin ZIP file from https://milliondollarscript.com/plugin
2. **Log in** to your WordPress admin dashboard
3. **Navigate** to Plugins > Add New
4. **Click** "Upload Plugin" at the top of the page
5. **Choose** the downloaded ZIP file and click "Install Now"
6. **Activate** the plugin after installation completes

After activation, the plugin automatically registers routes, flushes permalinks, sets up options, schedules cron jobs, and creates admin pages. You should now see "Million Dollar Script" in your WordPress admin menu.

If Million Dollar Script 2 is installed, activation redirects to the Million Dollar Script setup screen and shows the upgrade choice. Million Dollar Script does not migrate old data or deactivate Million Dollar Script 2 unless you explicitly choose that action. See [Upgrade From Million Dollar Script 2](/docs/mds-3/million-dollar-script/3.0.0/main/mds3-upgrade-from-mds2).

**Note:** Permalinks are automatically flushed during activation. If you experience 404 errors on Million Dollar Script routes after activation, manually flush permalinks by visiting Settings > Permalinks and clicking "Save Changes".

**Note:** If you encounter any errors during installation, see the [Troubleshooting Guide](/docs/mds-3/million-dollar-script/3.0.0/main/troubleshooting#installation-issues).

### Advanced Installation (Manual)

If you prefer manual installation or need to install via FTP/SSH:

- Extract the plugin ZIP and upload the `million-dollar-script` folder to `wp-content/plugins/`
- Activate the plugin from the WordPress Plugins page

The plugin will automatically flush permalinks on activation. If needed, you can manually flush via Settings > Permalinks (save) or WP‑CLI: `wp rewrite flush --hard`

## Troubleshooting

Having issues with installation, updates, or configuration? See the comprehensive [Troubleshooting Guide](/docs/mds-3/million-dollar-script/3.0.0/main/troubleshooting) which covers:

- [Installation Issues](/docs/mds-3/million-dollar-script/3.0.0/main/troubleshooting#installation-issues) - Upload limits, activation errors, permissions
- [Plugin Updates](/docs/mds-3/million-dollar-script/3.0.0/main/troubleshooting#plugin-updates) - Download failures, update errors
- [Routes & 404s](/docs/mds-3/million-dollar-script/3.0.0/main/troubleshooting#routes--404-errors) - Permalink issues
- [Grid Alignment](/docs/mds-3/million-dollar-script/3.0.0/main/troubleshooting#grid-alignment) - Selection boxes not matching grid

## First Steps

Use the Setup Wizard to get the basics in place, then decide whether you prefer pretty routes or page-based embeds.

### Run the Setup Wizard

- Go to Million Dollar Script > Setup Wizard in WP Admin.
- Let the wizard create the standard pages (Grid, Order, Manage, List, etc.).
- Save Options at the end so dynamic CSS is generated and settings persist.

You can adjust or delete any created page later and use blocks/shortcodes as needed.

- Pretty routes (no pages): visit `/milliondollarscript/order`, `/milliondollarscript/manage`, `/milliondollarscript/list`, `/milliondollarscript/payment`, `/milliondollarscript/thank-you`, etc. The base segment is configurable in Options.
- Page-based flow (optional): add the Million Dollar Script block or shortcode to your pages. This lets you use your theme’s templates, builders, and menus.

### Recommended Pages (optional)

Create pages and insert the **Grid Embed** block, or use the `[mds_grid ...]` shortcode. Legacy `[milliondollarscript ...]` embeds are handled after Million Dollar Script 2 is inactive or after migration.

- Grid
  - Block type: Grid
  - Shortcode: `[mds_grid id="1" read_only="false"]`
  - Notes: the grid width is responsive by default. Use the block Width control or `width="960px"` to constrain the maximum display width. Use `renderer="classic"` for the classic canvas fallback, or leave `renderer` unset for the default automatic renderer.

- Buy Pixels / Order
  - Block type: Order Pixels (or Write Your Ad, then Confirm Order)
  - Shortcode: `[mds3_page type="order" grid_id="1"]`
  - Notes: Million Dollar Script starts ordering from the interactive grid. Legacy `type="users"` pages migrate to the closest supported flow.

- List (Advertisers)
  - Block type: Ads List
  - Shortcode: `[mds3_page type="list" grid_id="1"]`
  - Notes: When no `id` is provided, the list shows all grids.

- Stats Box (optional)
  - Block type: Stats box
  - Shortcode: `[mds3_page type="stats" grid_id="1"]`
  - Notes: shows sold and available inventory. Options > Stats Display Mode chooses pixels or blocks by default, and the Stats block can override that per embed.

- Manage Pixels (optional public page)
  - Block type: Manage Pixels
  - Shortcode: `[mds3_page type="manage" grid_id="1"]`

- Payment and Thank‑You (optional as pages)
  - Block types: Payment, Thank‑You
  - Shortcodes:
    - `[mds3_page type="payment" grid_id="1"]`
    - `[mds3_page type="thank-you" grid_id="1"]`
  - Notes: With an order id/key, these pages show the order summary and a payment link when WooCommerce or a standalone Checkout URL is configured.

## Options & Styling

- Options live under “Million Dollar Script > Options”.
- Theme and color settings generate a dynamic CSS file stored under uploads. Saving Options regenerates it. You can also regenerate via WP‑CLI if needed:
  - `wp eval '\\MillionDollarScript\\Classes\\Web\\Styles::save_dynamic_css_file(); echo "OK\n";'`
- Pixel permalink base and slug pattern are configurable (see Options > Pixels). The `mds-pixel` post type pages are optionally enabled.

## Update Channels

Million Dollar Script uses the same branch names as the release workflow:

- `main` is the stable release channel.
- `beta` is for pre-release testing.
- `alpha` is for active development builds.

Older saved values are normalized automatically: `stable` maps to `main`, and `development` maps to `alpha`.

## Payment Provider (optional)

- Choose a payment provider under Million Dollar Script > Setup. Standalone/manual checkout is built in.
- For WooCommerce, install WooCommerce, activate **Million Dollar Script WooCommerce Checkout**, then choose WooCommerce as the provider. Million Dollar Script creates linked WooCommerce orders and sends customers to WooCommerce checkout after artwork upload.
- There’s an optional login redirect (Options > Login) to control where users land after WooCommerce login.
- For standalone/manual payment pages, use the Checkout URL option. Million Dollar Script supports the Million Dollar Script 2 placeholder format and falls back to the thank-you/order-summary page when no checkout URL is configured. See [Million Dollar Script Checkout And Payments](/docs/mds-3/million-dollar-script/3.0.0/main/mds3-checkout-and-payments).

## Notes & Tips

- Grid dimensions: define grid width/height and block size under Million Dollar Script > Dashboard > Grids. Million Dollar Script stores only sold, reserved, unavailable, custom-priced, or media-bearing blocks, so large grids remain manageable.
- Packages, price zones, order caps, unavailable regions, and renderer defaults are managed under Million Dollar Script > Dashboard > Grids > Edit Grid. See [Million Dollar Script Grid Pricing And Renderers](/docs/mds-3/million-dollar-script/3.0.0/main/mds3-grid-pricing-and-renderers).
- Theme Mode controls both Million Dollar Script admin screens and frontend grid/page embeds. Use Light or Dark for a fixed appearance, or System to follow the visitor's device preference.
- ImageGrid remote rendering is optional. Million Dollar Script uses local rendering until the Million Dollar Script ImageGrid extension is active; then configure API and quota settings under Settings > Rendering and review account/fallback status under Extensions. See [ImageGrid](/docs/mds-3/million-dollar-script/3.0.0/main/imagegrid).
- Multiple grids per page are supported. The UI and tooltips are scoped so clicks and popups map to the correct grid.
- If you switch your endpoint base (Options > Routes), flush permalinks.
- If block selections don't align with the image, see [Troubleshooting: Grid Alignment](/docs/mds-3/million-dollar-script/3.0.0/main/troubleshooting#grid-alignment).
