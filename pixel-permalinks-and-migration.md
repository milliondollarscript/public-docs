# Pixel Permalinks & Migration

MDS exposes an `mds-pixel` post type for individual advertiser pixels. You can enable public pixel pages and customize their URLs.

## Pixel pages (on/off)

- Options → Pixels → “Enable MDS Pixel pages”
- When disabled, visiting a single pixel URL returns a 404 (by design).
- When enabled, the plugin loads `templates/mds-pixel/single-mds-pixel.php`, or a theme override at `mds-pixel/single-mds-pixel.php` if present.

## Permalink base and slug pattern

- Base segment: default `mds-pixel`, configurable in Options.
- Slug structure: tokenized pattern you can customize, e.g. `%username%`, `%display_name%`, `%order_id%`, `%grid%`, `%pixel_id%`, `%text%`, `%meta:field%`.
- Developers can hook filters to add tokens or clamp lengths.

## Migration tool

- If you change the base or the slug pattern, run the migration from the Options screen.
- The tool updates existing pixel slugs in batches, preserves previous slugs, and registers automatic 301 redirects from legacy structures.
- Good practice: Save Options first, then run the migration.

## Search behavior

- When enabled, pixel titles and popup text are searchable in WordPress while results remain limited to completed pixels.

## Upgrading from legacy 2.3.5

- Earlier setups didn’t have the same permalink flexibility. After upgrading, set your preferred base/pattern and run the migration.
- The tool preserves old links via redirects, so external backlinks continue to work.

