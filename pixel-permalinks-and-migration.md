---
slug: pixel-permalinks-and-migration
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [site-owners, administrators]
published: true
tags: [urls, redirects, migration, compatibility]
---

# URLs, Redirects, and Legacy Pixel Pages

MDS 3.0 uses standard WordPress pages for the public grid, ordering, upload, management, list, statistics, terms, and privacy workflows. The setup wizard can create or update those pages and store their assignments.

## Current Pages

Open **Million Dollar Script > Settings > URLs & Redirects** to review page assignments and destination behavior. Use the block editor or current shortcodes to place a specific grid. When several grids exist, selectors must remain searchable and paginated rather than assuming a single grid.

After changing pages or rewrite-sensitive settings, visit **Settings > Permalinks** and save once if WordPress has not refreshed its rewrite rules.

## Legacy Pixel URLs

MDS 2 could generate individual advertiser pixel pages and custom slug patterns. MDS 3.0 retains selected settings as compatibility data for migration and custom code, but they are not primary current workflow controls.

During migration:

1. Run the dry run and review detected page IDs and URL settings.
2. Keep MDS 2 active until old links and current pages have been compared on staging.
3. Create explicit WordPress redirects for legacy URLs that must remain indexed or bookmarked.
4. Preserve query parameters only when they are required and safe.
5. Test logged-out, logged-in customer, and administrator destinations.

Do not enable URL cloaking solely because MDS 2 used it. Choose redirects based on the current page flow and avoid concealing external checkout destinations from customers.

For the original MDS 2 permalink settings, switch to the MDS 2 documentation version.
