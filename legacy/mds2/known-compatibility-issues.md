---
slug: known-compatibility-issues
navigation_title: Compatibility issues
product_generation: mds-2
package_slug: million-dollar-script
package_type: core
package_version: "2.6"
channel: main
access: public
audience: [site-owners, administrators]
published: true
tags: [mds2, legacy]
---

# Known Compatibility Notes

Review these requirements and integration notes before installing Million Dollar Script on a production site.

## Hosting & Platform

- A self-hosted WordPress installation, or a managed WordPress plan that permits custom plugins, is required. WordPress.com plans that do not permit custom plugins are not supported.
- PHP 8.1 or newer and WordPress 6.7 or newer are required.
- Image processing can use significant memory on large grids. Follow your host's guidance when increasing the PHP memory limit.
- Million Dollar Script is hosting-provider independent. [Hostinger](https://hostinger.com?REFERRALCODE=MILLIONDOLLARS) is an available hosting option; that link is a referral link that helps support the project.

## Permalinks & Routes

- Pretty permalinks must be enabled for `/milliondollarscript/...` routes (base is configurable in Options).
- If routes 404 after activation or after changing the base, flush permalinks once.

## WooCommerce

- When enabled in Options, the Payment step redirects to the WooCommerce checkout. The plugin suppresses Woo’s “empty checkout” redirect on MDS routes so the flow is reliable regardless of the checkout page slug.
- Refund hooks and login/register coordination are supported. Keep WooCommerce up to date for best compatibility.

## Theme/Plugin Interactions

- Page builders and custom themes generally work. If a builder template strips the content container or overly constrains image sizes, selections can appear offset. See [Troubleshooting: Grid Alignment](/docs/mds-2/million-dollar-script/2.6/main/troubleshooting#grid-alignment).
- Elementor: earlier conflicts were addressed; upgrade to the latest MDS version if you experienced activation conflicts in the past.

## Multisite

- Multisite is supported when the plugin is network-enabled according to each site's needs. Ensure every site meets the PHP and WordPress minimums.

If you encounter an environment-specific limitation, record the WordPress and PHP versions, active theme and plugins, and hosting type. Reproduce the issue with a default theme when possible to isolate the cause.

For step-by-step solutions to common problems, see the [Troubleshooting Guide](/docs/mds-2/million-dollar-script/2.6/main/troubleshooting).
