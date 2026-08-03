---
slug: known-compatibility-issues
navigation_title: Compatibility issues
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [site-owners, administrators]
published: true
---

# Known Compatibility Notes

Review these requirements and integration notes before installing Million Dollar Script on a production site.

## Hosting & Platform

- A self-hosted WordPress installation, or a managed WordPress plan that permits custom plugins, is required. WordPress.com plans that do not permit custom plugins are not supported.
- PHP 8.1 or newer, WordPress 6.0 or newer, and a PHP memory limit of at least 256 MB are required.
- Image processing can use significant additional memory on large grids. Use hosted ImageGrid rendering for workloads that are not reliable on the available shared-host resources.
- Million Dollar Script is hosting-provider independent. [Hostinger](https://hostinger.com?REFERRALCODE=MILLIONDOLLARS) is an available hosting option; that link is a referral link that helps support the project.

## Permalinks & Routes

- Pretty permalinks must be enabled for `/milliondollarscript/...` routes (base is configurable in Options).
- If routes 404 after activation or after changing the base, flush permalinks once.

## WooCommerce

- When the WooCommerce provider extension is active and selected in Setup, the Payment step redirects to WooCommerce checkout.
- Refund hooks and login/register coordination are supported. Keep WooCommerce up to date for best compatibility.

## Theme/Plugin Interactions

- Page builders and custom themes generally work. If a builder template strips the content container or overly constrains image sizes, selections can appear offset—see [Troubleshooting: Grid Alignment](/docs/mds-3/million-dollar-script/3.0.0/main/troubleshooting#grid-alignment).
- Elementor: earlier conflicts were addressed; upgrade to the latest MDS version if you experienced activation conflicts in the past.

## Multisite

- Multisite is supported when the plugin is network-enabled according to each site's needs. Ensure every site meets the PHP and WordPress minimums.

If you encounter an environment-specific limitation, record the WordPress and PHP versions, active theme and plugins, and hosting type. Reproduce the issue with a default theme when possible to isolate the cause.

For step-by-step solutions to common problems, see the [Troubleshooting Guide](/docs/mds-3/million-dollar-script/3.0.0/main/troubleshooting).
