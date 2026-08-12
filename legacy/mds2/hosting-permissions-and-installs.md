---
slug: hosting-permissions-and-installs
navigation_title: Hosting and permissions
product_generation: mds-2
package_slug: million-dollar-script
package_type: core
package_version: "2.6"
channel: main
access: public
audience: [site-owners, administrators]
published: true
tags: [mds2, hosting, permissions, installation, troubleshooting]
---

# Hosting, Permissions, and Installation Troubleshooting

The supported MDS 2 package is a WordPress plugin. Install and maintain WordPress first, then install Million Dollar Script through WordPress. Historical cPanel and Yahoo installation articles described the retired standalone script and included permissions and database steps that are not appropriate for the WordPress plugin.

## Current hosting requirements

Use a host that meets the requirements in [Installation and Setup](/docs/mds-2/million-dollar-script/2.6/main/installation-and-setup). Confirm the required PHP extensions, HTTPS, scheduled tasks, database capacity, and enough memory for WordPress image processing.

Shared hosting can work for a modest grid. Larger grids, many active advertisements, or high order volume may need more memory, CPU time, database capacity, and a persistent object cache.

## Install through cPanel or another hosting panel

1. Create or select a maintained WordPress installation in the host's WordPress manager.
2. Confirm HTTPS and a supported PHP version.
3. Upload the Million Dollar Script ZIP through **Plugins > Add New > Upload Plugin**, or use the approved updater from an existing installation.
4. Activate the plugin and complete its setup flow.
5. Configure a real system cron to call WordPress cron if the host's traffic is too low for reliable scheduled processing.

Do not follow old directions that create separate MDS database tables manually, use a default administrator password, or expose standalone installation scripts.

## File permissions

Use the ownership and permissions recommended by WordPress and your host. A typical setup uses directories readable and traversable by the web server and files readable by it, while granting write access only where WordPress needs it.

Do not set the whole site, plugin directory, or uploads directory to `777`. World-writable permissions can expose the site to modification by other users or compromised processes. If Million Dollar Script cannot write an image or cache file, fix ownership and the narrow target directory instead.

For a host-specific configuration, follow the host's documentation or the [WordPress file permissions guide](https://developer.wordpress.org/advanced-administration/server/file-permissions/).

## Memory exhausted

A PHP memory error means one request exceeded its configured limit. Record the exact limit and failing operation, then check image dimensions, grid dimensions, active plugins, and the host's process limits.

Reducing oversized source images and disabling an unnecessary conflicting plugin may help. If the workload is legitimate, raise the WordPress and PHP memory limits within the hosting plan or move to a plan with more resources. Repeatedly retrying the same request can make resource pressure worse.

## Installation notices and warnings

The current WordPress plugin does not use the retired standalone `install.php` workflow. If warnings appear during WordPress activation:

1. Confirm the plugin, WordPress, and PHP versions are supported.
2. Check **Tools > Site Health** and the PHP error log.
3. Enable WordPress debug logging on a non-production copy when more detail is needed.
4. Correct the reported dependency, permission, or database problem before retrying.

Do not hide a fatal error by disabling all error reporting. Production sites should avoid displaying debug output to visitors, but should retain private error logging.

## Archived standalone MDS 2

The standalone package is preserved for existing historical installations, but its old hosting-panel tutorials are not a safe installation baseline. Keep an isolated backup before maintaining one and plan a migration to a supported WordPress release.

MDS 3.0 has different requirements and migrations. Use the requirements page for the installed MDS 3.0 version instead of applying this MDS 2 guide.
