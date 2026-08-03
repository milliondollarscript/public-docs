---
slug: troubleshooting
summary: Resolve common Million Dollar Script installation, route, rendering, extension, update, checkout, API, and email issues.
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [site-owners, administrators]
published: true
tags: [troubleshooting, updates, extensions, checkout, security]
---

# Troubleshooting

Use this guide when installation, routes, extensions, checkout, rendering, updates, documentation, or emails do not behave as expected. If the issue is not covered here, review the [Known Compatibility Notes](/docs/mds-3/million-dollar-script/3.0.0/main/known-compatibility-issues) or contact support with the relevant error message and environment details.

## Installation

### “The link you followed has expired”

The plugin ZIP may exceed the upload limit applied to the current PHP request.

1. Check the maximum upload size under **Media > Add New**.
2. Increase `upload_max_filesize` and `post_max_size` through your hosting control panel or ask the hosting provider to change them.
3. If browser upload is unavailable, extract the package and upload the `million-dollar-script` directory to `wp-content/plugins/` using your host's file manager, SFTP, or SSH.

Avoid changing `.htaccess`, `php.ini`, or `wp-config.php` unless your hosting provider documents that method. A hosting-level limit can override values placed in WordPress files.

### Activation fails with a fatal error

- Confirm the site runs WordPress 6.0 or newer and PHP 8.1 or newer.
- Confirm the extracted directory contains the complete plugin package.
- Review `wp-content/debug.log`, the hosting error log, or the fatal-error email from WordPress.
- Record the complete error, PHP version, WordPress version, and active plugin list before contacting support.

### Installation cannot create a directory

Confirm that WordPress can write to `wp-content/plugins/`. Typical permissions are `755` for directories and `644` for files, but the correct ownership and permissions depend on the host. Do not make the directory world-writable.

## PHP Memory

Million Dollar Script requires an effective PHP memory limit of at least 256 MB for supported operation with checkout and extensions. Check the applied value under **Million Dollar Script > System Status** or **Tools > Site Health**; the hosting plan maximum can differ from the value assigned to an individual WordPress request.

If PHP reports an allowed-memory-size error:

- Increase the effective `memory_limit` to at least 256 MB through the hosting control panel or hosting support.
- Confirm the new value in System Status rather than assuming a configuration file was applied.
- Update WordPress, Million Dollar Script, the active payment provider, and active extensions.
- Reproduce the request on staging with unrelated plugins disabled.
- Check whether a bulk operation, report, migration, or image job is processing an unexpectedly large data set.
- Use hosted ImageGrid rendering when large-grid processing is not reliable on the available server resources.

The filename and line in a memory fatal identify the final failed allocation, not necessarily the component that consumed most of the request memory. Continued growth after increasing the limit should be investigated as an unbounded operation.

## Routes and 404 Errors

### Routes return 404 after activation

Refresh WordPress rewrite rules:

1. Go to **Settings > Permalinks**.
2. Click **Save Changes** without changing the permalink structure.
3. Clear any page, object, proxy, and CDN caches.

With WP-CLI, run:

```bash
wp rewrite flush --hard
```

If only one route fails, check for a WordPress page, attachment, taxonomy, or other plugin using the same slug. Review the configured route on the Million Dollar Script settings and setup screens before changing it.

## Grid Alignment and Rendering

If selection highlights do not align with the grid image:

- Confirm the block or shortcode points to the intended grid.
- Confirm grid dimensions and block size match the generated image.
- Save the grid after changing dimensions, backdrop, or rendering settings so derived assets can be regenerated.
- Check whether the theme or page builder scales the image independently of its selection layer.
- Ensure containers do not constrain the image while leaving the overlay at its original width.
- Clear page and CDN caches after regenerating grid assets.

If several grids appear on one page, verify each block or shortcode uses the correct grid identifier and inspect the browser console for JavaScript errors.

If grids load slowly or tiles are missing:

- Confirm the configured renderer mode.
- Confirm generated files exist before public pages request them.
- Check browser console and network requests for 404 responses or slow WordPress AJAX requests.
- If local PNG tile requests return `404` or an image workflow reports an undefined `imagecreate()` function, follow the [PHP GD troubleshooting guide](/docs/mds-3/million-dollar-script/3.0.0/main/fatal-error-call-to-undefined-function-imagecreate).
- Use hosted ImageGrid rendering for very large grids or processing that exceeds shared-host limits.

## Extension Catalog

The WordPress.org edition does not install extension ZIP files from the Million Dollar Script service. Use **Discover extensions** to browse compatible products, install free extensions through **Plugins > Add New**, and upload premium packages supplied with a purchase.

For the direct-download edition, if the catalog does not load:

- Confirm **Extension Server URL** under **Million Dollar Script > Settings**.
- Confirm the server is reachable from WordPress and is not blocked by DNS, TLS, firewall, or hosting restrictions.
- Confirm local development points to the intended local extension server.
- Confirm the catalog is returning packages compatible with the installed core generation and API version.

Custom clients that call the extension service directly must preserve the compatibility signals sent by core: `product_family=modern`, `core_version=<installed version>`, `core_api_version=1`, and the transitional `mds_generation=3` marker.

## Extension Installation and Activation

If an extension fails to install or does not appear:

- Confirm WordPress can write to `wp-content/plugins/`.
- Confirm the package supports the installed Million Dollar Script generation and version.
- Confirm the extension is activated under **Plugins > Installed Plugins**.
- For premium extensions, confirm the license is active and grants access to that package.
- Review the WordPress debug log and hosting error log for ZIP, permission, dependency, or PHP errors.

To isolate an extension conflict on staging:

1. Deactivate all Million Dollar Script extensions.
2. Confirm core works by itself.
3. Reactivate extensions one at a time.
4. Record the first failing combination, along with WordPress, PHP, core, extension, and theme versions.

## Updates

### “Download failed: Forbidden”

Signed update URLs are time-limited. Refresh the WordPress Updates screen or check for updates again, then retry immediately so WordPress receives a new URL.

If an update still appears after installation:

- Refresh the WordPress Updates and Plugins screens.
- Confirm the installed version under **Plugins > Installed Plugins**.
- Confirm the expected update channel is selected.
- Clear stale plugin ZIP or response caches on development servers and proxies.
- If necessary, download the current package and reinstall it manually without deleting its saved data.

## Documentation

Million Dollar Script keeps remotely delivered extension documentation in a short-lived local cache and clears affected entries when license, extension-pack, or tester access changes.

If a guide remains outdated after a documentation release, use **Million Dollar Script > Documentation > Refresh documentation**. This clears the site's remote-documentation cache and retrieves the current guides allowed by its access. It does not change licenses, install extensions, edit server content, or bypass entitlement checks.

Manual refreshes have a short site-wide cooldown. Use the action after a documentation release, an entitlement change, or recovery from a temporary extension-server problem rather than as routine maintenance.

## Checkout and WooCommerce

If checkout fails or does not redirect:

- Confirm a payment provider is selected and reports ready.
- When using WooCommerce, confirm WooCommerce and the Million Dollar Script WooCommerce Checkout extension are active.
- Confirm the WooCommerce checkout page exists and is published.
- Confirm the order is still payable or renewable and review its current status.
- Review WooCommerce scheduled actions and logs for delayed or failed work.
- Clear caches after changing payment-provider or checkout settings.

If WooCommerce redirects an empty cart away from a Million Dollar Script checkout route, update core, WooCommerce, and the checkout extension before further diagnosis.

## API Access

If an API endpoint cannot be made public or less restrictive, check its minimum security level under **Million Dollar Script > API Access**. Policy choices weaker than the endpoint minimum are disabled so sensitive write, management, and service routes cannot be exposed accidentally.

API clients should send `Authorization: Bearer ...` or `X-Million-Dollar-Script-API-Key`. Browser write endpoints governed by the public-write nonce policy also require a valid WordPress REST nonce in `X-WP-Nonce`.

## Styling

If styles appear incorrect after an update:

- Clear browser, WordPress, proxy, and CDN caches.
- Test the affected page with a default WordPress theme to isolate theme or page-builder rules.
- Check whether global `img`, form, button, or container rules override Million Dollar Script components.
- Inspect the browser console for missing assets and Content Security Policy errors.

Avoid broad custom CSS that scales the grid image independently of its overlay or changes every image inside the Million Dollar Script container.

## Diagnostics and Scheduled Work

- Review **Million Dollar Script > System Status** for PHP, WordPress, memory, database, and integration health.
- Enable WordPress debugging on staging when a reproducible PHP error needs investigation.
- Inspect `wp-content/debug.log`, hosting logs, WooCommerce logs, and scheduled actions as appropriate.
- Use `wp cron event list` to confirm WordPress cron is running before manually executing a Million Dollar Script event.

Do not publish logs without removing customer data, license keys, API credentials, signed URLs, cookies, and server paths that should remain private.

## Emails

Million Dollar Script sends order email through WordPress mail and does not maintain a separate delivery log. If messages are missing:

- Install an SMTP or mail-logging plugin and send a test message.
- Confirm the site sender address is accepted by the configured mail service.
- Check spam filtering, provider suppression lists, and DNS records used for mail authentication.
- Confirm the related order event completed before expecting the message.

## Still Need Help?

Before contacting support, collect:

- The exact error and time it occurred.
- WordPress, PHP, Million Dollar Script, payment-provider, and extension versions.
- The affected URL or workflow.
- Relevant sanitized log entries.
- Whether the issue reproduces with a default theme and unrelated plugins disabled on staging.

Use the [contact page](https://milliondollarscript.com/contact/) for support or professional installation assistance.
