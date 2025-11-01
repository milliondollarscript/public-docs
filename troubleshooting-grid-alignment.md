# Troubleshooting: Grid Alignment

Some troubleshooting tips for grid alignment.

If block highlights or selection boxes don’t align with the grid image, check the following:

## 1) Match grid dimensions

- In MDS Admin > Manage Grids, confirm your grid width/height and block size.
- For the Grid view:
  - Block users: set width/height to `{width}`/`{height}` so the block calculates exact dimensions from the database.
  - Shortcode users: ensure `width` and `height` match the grid’s pixel size (grid_width × block_width, grid_height × block_height). Example: `[milliondollarscript id="1" type="grid" width="1000px" height="1000px"]`.

## 2) Use the right display types

- Grid: `type="grid"`
- Ordering flow: `type="users"` (legacy) or split into `type="order"`, `type="write-ad"`, and `type="confirm-order"`
- List/Manage/Stats: `list`, `manage`, `stats` generally use `width="100%" height="auto"`

## 3) Regenerate images when grid settings change

- Changing grid/backdrop settings requires the grid image to be updated. The plugin handles regeneration automatically, but if you made changes and still see a mismatch, save the grid again to trigger a refresh and retest.

## 4) Keep CSS scaling in check

- Avoid custom CSS that forces the grid image to scale independently of the selection layer.
- If your theme applies global `img { max-width: 100%; height: auto; }`, ensure the container layout still preserves the grid’s intended size.

## 5) Multiple grids on one page

- Multiple grids are supported. If selections affect the wrong grid, ensure each block/shortcode has the correct `id` and that you haven’t nested containers that clamp widths unexpectedly.

## 6) Clear caches

- After changing grid settings or images, clear any page/CDN caches to eliminate stale assets.

If issues persist, confirm that dynamic CSS is up to date (save Options to regenerate) and that your route/page isn’t overridden by a theme template with conflicting styles.

