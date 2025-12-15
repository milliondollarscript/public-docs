# Troubleshooting

This guide consolidates common issues and their solutions. If you don't find your answer here, check the [Known Compatibility Notes](known-compatibility-issues.md) or contact support.

## Installation Issues

### "The link you followed has expired"

**Cause:** The plugin ZIP file exceeds your server's upload file size limits.

**Solutions:**

1. **Check your current limits:** Go to Media > Add New in WordPress admin. The maximum upload size is displayed at the bottom.

2. **Increase PHP limits** in `php.ini`:
   ```
   upload_max_filesize = 64M
   post_max_size = 64M
   max_execution_time = 300
   ```

3. **Alternative methods:**
   - **Apache `.htaccess`:** Add to your WordPress root:
     ```
     php_value upload_max_filesize 64M
     php_value post_max_size 64M
     php_value max_execution_time 300
     ```
   - **`wp-config.php`:** Add before "That's all, stop editing!":
     ```php
     @ini_set('upload_max_filesize', '64M');
     @ini_set('post_max_size', '64M');
     ```

4. **Contact hosting provider:** Many hosts have control panel options to adjust limits.

5. **Manual installation:** Upload via FTP/SSH instead. Extract the ZIP and upload the `milliondollarscript-two` folder to `wp-content/plugins/`.

### Plugin Activation Errors

**"Plugin could not be activated because it triggered a fatal error"**

This usually indicates PHP version incompatibility or missing dependencies.

**Solutions:**
- Verify your server meets minimum requirements (PHP 8.1+, WordPress 6.7+)
- Check error logs (`wp-content/debug.log` or hosting control panel)
- Contact support with the specific error message

### "Missing plugin files" or "Corrupted ZIP"

**Cause:** The ZIP file was incompletely downloaded or corrupted.

**Solutions:**
- Re-download from https://milliondollarscript.com/plugin
- Verify the download completed fully (check file size)
- Try a different browser
- Use manual FTP/SSH installation

### Permission Errors

**"Could not create directory" or "Installation failed"**

**Cause:** WordPress doesn't have write permissions to the plugins directory.

**Solutions:**
- Contact hosting provider to verify permissions on `wp-content/plugins/`
- Recommended: directories `755`, files `644`
- Use manual installation via FTP/SSH

---

## Plugin Updates

### "Download failed: Forbidden"

**Cause:** The download link expired before you clicked "Update Now". Update download links are time-limited for security.

**Solution:** Simply retry the update. WordPress will fetch a fresh download link. If you see this error repeatedly, try:
1. Click "Check for updates" to refresh the update information
2. Immediately click "Update Now"
3. If the issue persists, manually download the latest version from https://milliondollarscript.com/plugin and reinstall

---

## Routes & 404 Errors

### Routes return 404 after activation

**Cause:** WordPress permalinks need to be flushed.

**Solutions:**
1. Go to Settings > Permalinks and click "Save Changes" (no changes needed)
2. Or via WP-CLI: `wp rewrite flush --hard`

### Routes 404 after changing the endpoint base

If you changed the route base in Options (e.g., from `/milliondollarscript/` to `/pixels/`):

1. Save Options
2. Flush permalinks (Settings > Permalinks > Save)
3. Clear any page caches

---

## Grid Alignment

If block highlights or selection boxes don't align with the grid image:

### 1) Match grid dimensions

- In MDS Admin > Manage Grids, confirm your grid width/height and block size
- **Block users:** Set width/height to `{width}`/`{height}` for automatic sizing
- **Shortcode users:** Ensure dimensions match exactly (grid_width × block_width, grid_height × block_height)
  ```
  [milliondollarscript id="1" type="grid" width="1000px" height="1000px"]
  ```

### 2) Use correct display types

- Grid: `type="grid"`
- Ordering: `type="users"` (legacy) or split into `order`, `write-ad`, `confirm-order`
- List/Manage/Stats: Use `width="100%" height="auto"`

### 3) Regenerate images after changes

When grid or backdrop settings change, the grid image updates automatically. If misalignment persists, save the grid again to trigger a refresh.

### 4) Check CSS scaling

- Avoid custom CSS that scales the grid image independently of the selection layer
- If your theme applies `img { max-width: 100%; height: auto; }`, ensure the container preserves the grid's intended size

### 5) Multiple grids on one page

Multiple grids are supported. If selections affect the wrong grid:
- Ensure each block/shortcode has the correct `id`
- Check that containers aren't unexpectedly clamping widths

### 6) Clear caches

After changing grid settings or images, clear page/CDN caches to eliminate stale assets.

---

## WooCommerce Issues

### Payment redirect not working

**Solutions:**
- Verify WooCommerce's checkout page is configured and published
- Ensure you're logged in and an order ID is present
- Confirm WooCommerce integration is enabled in MDS Options > WooCommerce
- Clear caches after changing Woo settings

### Empty checkout redirect conflicts

The plugin automatically suppresses WooCommerce's "empty checkout" redirect on MDS routes. If you experience conflicts:
- Update to the latest MDS version
- Keep WooCommerce up to date

---

## Styling Issues

### Styles appear wrong after update

**Solution:** Save Options once to regenerate the dynamic CSS file.

Or regenerate manually via WP-CLI:
```bash
wp eval '\\MillionDollarScript\\Classes\\Web\\Styles::save_dynamic_css_file(); echo "OK\n";'
```

### Theme conflicts with grid layout

- Ensure styles work within the `.mds-container` scope
- Check if page builders or themes are constraining image sizes
- Try the grid on a page using a default theme to isolate the issue

---

## Performance & Memory

### Memory errors during grid generation

Large grids require more memory for image processing.

**Solutions:**
- Increase PHP memory limit (consult hosting provider)
- Use smaller grid dimensions if possible
- Consider managed WordPress hosting optimized for image processing

---

## Debugging

### Enable logging

1. Go to MDS Options > System
2. Enable logging
3. Log files are stored in `wp-content/uploads/milliondollarscript/`

### Find and tail the current log

```bash
LOG_FILE=$(wp eval 'echo \\MillionDollarScript\\Classes\\System\\Logs::get_log_file_path();')
test -n "$LOG_FILE" && tail -f "$LOG_FILE"
```

### Run cron jobs manually

```bash
wp cron event list | grep milliondollarscript
wp cron event run milliondollarscript_cron_minute
```

---

## Extension Issues

### Extension not appearing in admin

**Cause:** The extension may not be activated, or may have a dependency issue.

**Solutions:**
- Verify the extension is activated in Plugins > Installed Plugins
- Ensure the MDS core plugin is active and up to date
- For premium extensions, verify your license is activated in the extension's settings
- Check PHP error logs for activation errors (`wp-content/debug.log` or hosting control panel)

### Extension features not working

**Solutions:**
- Clear any caching plugins (page cache, object cache)
- Ensure the extension version is compatible with your MDS core version
- For premium extensions, verify the license is valid and activated
- Try deactivating and reactivating the extension
- Check for JavaScript errors in your browser's developer console (F12)

### Extension conflicts

If an extension causes issues with MDS or other plugins:

1. Deactivate all MDS extensions
2. Verify the core MDS plugin works correctly
3. Reactivate extensions one at a time to identify the conflict
4. If a conflict is found, contact the extension developer with details about:
   - WordPress version
   - PHP version
   - MDS core version
   - Other active plugins/theme

### Extension update issues

**"Download failed" during extension update**

This is similar to core plugin updates—the download link may have expired.

**Solution:** Retry the update. If it persists, manually download the latest version from your MDS account and reinstall.

---

## Still Need Help?

- **MDS Installation Service:** Professional installation assistance at https://milliondollarscript.com
- **Hosting Support:** Your hosting provider can help with server configuration, PHP settings, and permissions
- **Isolate the issue:** Test with a default WordPress theme and no other plugins to rule out conflicts
