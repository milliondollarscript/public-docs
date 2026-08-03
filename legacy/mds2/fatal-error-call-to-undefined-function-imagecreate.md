---
slug: fatal-error-call-to-undefined-function-imagecreate
title: Fix “Call to undefined function imagecreate()” in PHP
navigation_title: PHP GD and image creation
summary: Diagnose a missing PHP GD extension, enable it for the correct web runtime, and verify imagecreate() for WordPress or a legacy Million Dollar Script 2 installation.
product_generation: mds-2
package_slug: million-dollar-script
package_type: core
package_version: "2.6"
channel: main
access: public
audience: [site-owners, administrators]
published: true
tags: [mds2, troubleshooting, php, gd, images]
---

# Fix “Call to undefined function imagecreate()” in PHP

This error means the PHP runtime handling the request does not have the GD extension available. Enable GD for that exact PHP version and web runtime, restart the relevant service, and verify the function again.

The diagnosis applies to PHP and WordPress generally, including legacy Million Dollar Script 2 installations. The original guidance was written for the standalone MDS 2 codebase; old PHP 4 instructions and `php_gd2.dll` steps do not describe a current WordPress or Million Dollar Script 3.0 setup.

## Confirm the missing extension

Run the check in the same PHP environment that serves the failing request. Command-line PHP and the PHP-FPM or Apache runtime can load different configuration files.

```bash
php -r 'var_dump(PHP_VERSION, extension_loaded("gd"), function_exists("imagecreate"));'
```

If `extension_loaded("gd")` or `function_exists("imagecreate")` returns `false`, GD is not available to that PHP process.

## Enable GD for the active PHP version

1. In WordPress, open **Tools > Site Health > Info > Media Handling**. If GD is active, WordPress reports its version and supported formats.
2. Confirm the PHP version assigned to the website.
3. Use your hosting control panel or ask your provider to enable the GD extension for that PHP version.
4. On a managed server or container, install or build the version-matched GD package, then restart PHP-FPM or the web server. Package names and restart commands vary by operating system and PHP version.
5. Repeat the check through the web runtime and retry the failed image operation.

## If GD appears enabled but the error remains

- Compare the PHP version shown by the website with the version returned by `php -v`.
- Check which configuration files each runtime loads. A module enabled for command-line PHP may still be absent from PHP-FPM.
- Restart the correct service after changing extensions, and clear any opcode or platform cache that retains the old worker process.
- Do not leave a public `phpinfo()` page online. It exposes detailed server configuration.

## Related resources

- [PHP `imagecreate()` manual](https://www.php.net/manual/en/function.imagecreate.php) — Current function signature, return value, and GD behavior.
- [PHP `extension_loaded()` manual](https://www.php.net/manual/en/function.extension-loaded.php) — How to verify whether an extension is loaded.
- [WordPress Site Health](https://wordpress.org/documentation/article/site-health-screen/) — Where WordPress reports its active image editor and GD capabilities.
- [MDS 2 troubleshooting](/docs/mds-2/million-dollar-script/2.6/main/troubleshooting) — Installation, routes, image generation, logs, and extension checks for supported MDS 2 WordPress releases.

_First published June 25, 2010. Reviewed and updated August 3, 2026._
