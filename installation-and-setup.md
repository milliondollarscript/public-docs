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

### Simple Installation (Recommended)

1. **Download** the latest plugin ZIP file from https://milliondollarscript.com/plugin
2. **Log in** to your WordPress admin dashboard
3. **Navigate** to Plugins > Add New
4. **Click** "Upload Plugin" at the top of the page
5. **Choose** the downloaded ZIP file and click "Install Now"
6. **Activate** the plugin after installation completes

After activation, the plugin automatically registers routes, flushes permalinks, sets up options, schedules cron jobs, and creates admin pages. You should now see "Million Dollar Script" in your WordPress admin menu.

**Note:** Permalinks are automatically flushed during activation. If you experience 404 errors on MDS routes after activation, manually flush permalinks by visiting Settings > Permalinks and clicking "Save Changes".

**Note:** If you encounter any errors during installation, please refer to the [Troubleshooting Installation Issues](#troubleshooting-installation-issues) section below.

### Advanced Installation (Manual)

If you prefer manual installation or need to install via FTP/SSH:

- Extract the plugin ZIP and upload the `milliondollarscript-two` folder to `wp-content/plugins/`
- Activate the plugin from the WordPress Plugins page

The plugin will automatically flush permalinks on activation. If needed, you can manually flush via Settings > Permalinks (save) or WP‑CLI: `wp rewrite flush --hard`

## Troubleshooting Installation Issues

### "The link you followed has expired"

**What causes this error:**
This error appears when uploading the plugin ZIP file if it exceeds your server's upload file size limits. WordPress enforces limits set in your PHP configuration.

**How to fix it:**

1. **Check your current limits:** Visit your WordPress admin dashboard and go to Media > Add New. The maximum upload size is displayed at the bottom of the page.

2. **Increase PHP limits:** You need to modify your `php.ini` file (or equivalent configuration). Add or update these settings:
   ```
   upload_max_filesize = 64M
   post_max_size = 64M
   max_execution_time = 300
   ```
   
3. **Alternative methods if you can't access php.ini:**
   - **`.htaccess` (Apache servers):** Add to your WordPress root `.htaccess` file:
     ```
     php_value upload_max_filesize 64M
     php_value post_max_size 64M
     php_value max_execution_time 300
     ```
   - **`wp-config.php`:** Add before "That's all, stop editing!" line:
     ```php
     @ini_set('upload_max_filesize', '64M');
     @ini_set('post_max_size', '64M');
     ```

4. **Contact your hosting provider:** Many hosts provide control panel options to adjust these limits, or support staff can increase them for you.

5. **Use manual installation:** If you cannot increase limits, use the [Advanced Installation (Manual)](#advanced-installation-manual) method instead by uploading via FTP/SSH.

### Plugin Activation Errors

**"Plugin could not be activated because it triggered a fatal error"**

This usually indicates a PHP version incompatibility or missing dependencies.

**Solutions:**
- Verify your server meets the minimum requirements (PHP 8.1+, WordPress 6.7+)
- Check your error logs (often in `wp-content/debug.log` or via your hosting control panel)
- Contact support with the specific error message for assistance

### "Missing plugin files" or "Corrupted ZIP"

**What causes this:**
The ZIP file may have been incompletely downloaded or corrupted during transfer.

**Solutions:**
- Re-download the plugin ZIP from https://milliondollarscript.com/plugin
- Verify the download completed fully (check file size)
- Try a different browser if the issue persists
- Use the manual installation method as an alternative

### Permission Errors

**"Could not create directory" or "Installation failed"**

This indicates WordPress doesn't have write permissions to the plugins directory.

**Solutions:**
- Contact your hosting provider to verify correct permissions on `wp-content/plugins/`
- Recommended permissions: directories `755`, files `644`
- Use manual installation via FTP/SSH if automatic installation isn't possible

### Need Additional Help?

- **MDS Installation Service:** Million Dollar Script offers a professional installation service for users who need assistance. Visit https://milliondollarscript.com for details
- **Hosting Support:** Your hosting provider can assist with server configuration, PHP settings, and file permissions
- **Check Documentation:** Review the WordPress Documentation for general plugin installation troubleshooting

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
