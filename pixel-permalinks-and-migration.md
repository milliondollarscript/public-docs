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

# Individual Advertiser Pages And Legacy Pixel URLs

MDS 3.0 uses standard WordPress pages for the public grid, ordering, upload, management, list, statistics, terms, and privacy workflows. The setup wizard can create or update those pages and store their assignments.

## Current Pages

Open **Million Dollar Script > Settings > URLs & Redirects** to review page assignments and destination behavior. Use the block editor or current shortcodes to place a specific grid. When several grids exist, selectors must remain searchable and paginated rather than assuming a single grid.

After changing pages or rewrite-sensitive settings, visit **Settings > Permalinks** and save once if WordPress has not refreshed its rewrite rules.

## Individual Advertiser Pages

Open **Million Dollar Script > Settings > URLs & Redirects** to enable a WordPress page for each active advertiser placement. The feature is disabled by default. Only active placements on active grids are published, and an associated order must be paid. Pending, unpaid, cancelled, expired, and archived placements remain private.

The URL base defaults to `mds-pixel` for continuity. New slugs default to the numeric placement ID so WordPress account names are not exposed. You can build slugs from placement ID, legacy pixel ID, order ID, grid, advertiser title, or popup text. Legacy username and display-name tokens deliberately resolve to blank in MDS 3.0.

Use **Exclude Advertiser Pages From Search** to keep direct pages available while adding `noindex` and removing them from WordPress search and XML sitemaps. The default page includes the placement image, public description, advertiser destination, and grid context. It never exposes customer email, WordPress account identity, order keys, or payment data.

Block themes can customize the Single template for the **Advertiser page** post type under **Appearance > Editor > Design > Templates > Manage templates > Add template**. Add or edit the Single item template for Advertiser page, then use ordinary template parts for the site header, footer, and surrounding layout. Classic and child themes can use normal single-post styling or add `million-dollar-script/single-advertiser.php`. An older `mds-pixel/single-mds-pixel.php` file is detected for review but never executed by MDS 3.0.

## Safe URL Changes

Changing the base retains up to 20 previous bases for exact 301 redirects. After changing the slug pattern, use **Preview slug migration** and explicitly confirm the permanent URL change. Migration runs in bounded batches and records the exact previous slug for WordPress redirects. Use **Synchronize advertiser pages** to repair missing proxy pages; large synchronizations continue in background cron batches.

The built-in grid popup can link to the full advertiser page. You can change its label and target. Custom popup layouts can use `%advertiser_page_url%` or `%advertiser_page_link%`; these placeholders change presentation only and never add customer fields.

Public pages use one canonical URL, WordPress robots controls, Open Graph metadata, and `WebPage` structured data. SEO Basic composes with this output instead of emitting duplicates and can include a dedicated advertiser sitemap section. Placement changes update the managed WordPress post and clear its post cache. If a separate CDN or full-page cache does not listen to WordPress post transitions, purge it after changing placement visibility.

## MDS 2 Migration

MDS 2 `mds-pixel` identities, current slugs, old-slug metadata, and base history are mapped to the corresponding placement page. Imported pages remain drafts until individual advertiser pages are enabled and the placements are public.

During migration:

1. Run the dry run and review detected page IDs and URL settings.
2. Keep MDS 2 active until old links and current pages have been compared on staging.
3. Enable individual advertiser pages only after reviewing which placements are public.
4. Synchronize the pages and test current and previous exact URLs.
5. Test logged-out, logged-in customer, and administrator destinations, canonical tags, robots output, and XML sitemaps.

Do not enable URL cloaking solely because MDS 2 used it. Choose redirects based on the current page flow and avoid concealing external checkout destinations from customers.

For the original MDS 2 permalink implementation, switch to the MDS 2 documentation version.
