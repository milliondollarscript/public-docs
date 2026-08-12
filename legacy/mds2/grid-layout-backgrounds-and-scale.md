---
slug: grid-layout-backgrounds-and-scale
navigation_title: Grid layout and backgrounds
product_generation: mds-2
package_slug: million-dollar-script
package_type: core
package_version: "2.6"
channel: main
access: public
audience: [site-owners, administrators]
published: true
tags: [mds2, grids, backgrounds, troubleshooting]
---

# Grid Layout, Backgrounds, and Scale

This guide covers common layout questions for the current Million Dollar Script WordPress plugin. Instructions written for the older standalone script may refer to template files or image-processing buttons that do not exist in the WordPress plugin.

## Set the grid size

A grid's visible size comes from its column count, row count, and block dimensions. Review those values in **Million Dollar Script > Manage Grids** before changing page or theme CSS.

For a 1,000 by 1,000 pixel grid made from 10 pixel blocks, use 100 columns and 100 rows. Larger grids create more inventory and require more browser rendering, database storage, and image-processing work.

After changing dimensions, save the grid and clear any page, object, and CDN caches. If the selection layer no longer matches the grid, follow the [grid alignment checklist](/docs/mds-2/million-dollar-script/2.6/main/troubleshooting-grid-alignment).

## Center the grid

Use the WordPress block editor's alignment controls when the theme supports them. For a shortcode or a theme that does not expose alignment controls, place the grid in a container with a constrained width and automatic left and right margins.

Do not use the obsolete HTML `center` element. Avoid CSS that scales only the grid image because the selection and link layers must use the same dimensions.

## Add a background image

Upload a web-optimized image through the grid editor. Match the image's intended canvas size to the grid where practical. A transparent PNG can preserve areas of the grid beneath it, while JPEG or WebP is usually smaller for a fully opaque photographic background.

Test contrast between the background, grid lines, and advertisements in both light and dark themes. Save the grid after changing its image, then clear caches if the previous image remains visible.

The old standalone MDS 2 instructions described uploading an image and manually processing pixels. Those steps apply only to the archived standalone package. The WordPress plugin manages generated grid assets through WordPress.

## Use more than one grid

Million Dollar Script does not impose a useful fixed limit such as 20 grids. The practical limit depends on grid dimensions, traffic, active orders, hosting resources, and how many grids appear on the same page.

Use a separate page or section for each major campaign. When placing multiple grids on one page, give each block or shortcode the correct grid ID and test the page on a phone as well as a desktop browser. Avoid loading dozens of large interactive grids in one view.

## Fix a JavaScript error

For the WordPress plugin:

1. Open the browser console and note the first error from Million Dollar Script, WordPress, the active theme, or another plugin.
2. Temporarily disable page minification or script deferral and clear caches.
3. Confirm that the grid image and its script requests return successful responses.
4. Test with a default WordPress theme to identify theme conflicts.
5. Re-save the grid and the page that embeds it.

An error about a missing `mouseover_box.htm` file belongs to the archived standalone script, not the WordPress plugin. Restore the matching file from the same standalone release instead of copying one from another version.

## Million Dollar Script 3.0

MDS 3.0 uses its own grid editor and rendering system. Do not apply MDS 2 template-file or database instructions to an MDS 3.0 installation. Use the documentation shown for the installed MDS 3.0 package and extensions.
