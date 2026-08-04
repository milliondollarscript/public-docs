---
slug: shortcode-reference
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [developers, administrators]
published: true
---

# Million Dollar Script Blocks And Shortcodes

Million Dollar Script provides native grid and stats blocks plus two primary shortcodes:

```text
[mds_grid id="1" read_only="false"]
[mds3_page type="order" grid_id="1"]
```

Legacy Million Dollar Script 2 embeds using `[milliondollarscript ...]` are supported when Million Dollar Script 2 is not active. If Million Dollar Script 2 remains active for side-by-side testing, Million Dollar Script leaves those legacy embeds to Million Dollar Script 2.

## Grid Embed Block

In the block editor, add **Grid Embed**. Choose:

- grid
- read-only or interactive mode
- width
- height
- renderer: `auto`, `openlayers`, or `classic`

The block renders dynamically, so changes to the grid record appear without editing the page again. The width defaults to `100%`, so it follows the page column unless you set a value such as `960px` or `80vw`.

If you deselect the block in the editor, click anywhere inside the grid preview or select it from the editor list view to reopen the block settings sidebar.

## Stats Widget Block

In the block editor, add **Stats Widget**. Choose:

- grid
- display unit: site setting, blocks, or pixels
- width
- number, label, background, and border colors

The stats block shows sold and available inventory. The site setting comes from **Settings > Display > Stats Display Mode**.

## `[mds_grid]`

Use `[mds_grid]` for the public grid itself.

### Attributes

- `id` (int): Million Dollar Script grid id. If omitted, Million Dollar Script uses the first available grid as a fallback.
- `read_only` (bool): `true` for showcase mode, `false` for ordering/selection mode. Default: `true`.
- `width` (string): CSS width such as `100%`, `960px`, or `80vw`. Default: `100%`.
- `height` (string): CSS height such as `640px`, `80vh`, or `100%`. Default: `640px`.
- `renderer` (string): `auto`, `openlayers`, or `classic`.

Renderer details, package pricing, and price zones are covered in [Million Dollar Script Grid Pricing And Renderers](/docs/mds-3/million-dollar-script/3.0.0/main/mds3-grid-pricing-and-renderers).

### Examples

Interactive order page:

```text
[mds_grid id="1" read_only="false"]
```

Constrained responsive width:

```text
[mds_grid id="1" read_only="false" width="960px" height="560px"]
```

Read-only showcase:

```text
[mds_grid id="1" read_only="true"]
```

Classic canvas fallback:

```text
[mds_grid id="1" read_only="false" renderer="classic"]
```

## `[mds3_page]`

Use `[mds3_page]` for standard Million Dollar Script pages and migrated Million Dollar Script 2 page roles.

Each page shortcode can target a specific grid. If you omit the grid id, Million Dollar Script uses the first available grid only as a fallback for that embed.

### Attributes

- `type` (string): one of `grid`, `order`, `write-ad`, `confirm-order`, `payment`, `manage`, `list`, `stats`, `thank-you`, `upload`, `no-orders`.
- `grid_id` (int): Million Dollar Script grid id. If omitted, Million Dollar Script uses the first available grid as a fallback.
- `width` (string): CSS size used by grid and stats embeds. Default: `100%` for grids, `240px` for stats.
- `height` (string): passed to the grid renderer when `type="grid"`.
- `renderer` (string): passed to the grid renderer when `type="grid"`.
- `unit` (string): for `type="stats"` only. Use `settings`, `blocks`, or `pixels`.
- `number_color` (hex color): for `type="stats"` only. Sets the sold/available number color.
- `label_color` (hex color): for `type="stats"` only. Sets the label color.
- `background_color` (hex color): for `type="stats"` only. Sets the stats panel background.
- `border_color` (hex color): for `type="stats"` only. Sets the stats panel border.

### Examples

```text
[mds3_page type="grid" grid_id="1"]
[mds3_page type="order" grid_id="1"]
[mds3_page type="manage" grid_id="1"]
[mds3_page type="list" grid_id="1"]
[mds3_page type="stats" grid_id="1"]
[mds3_page type="stats" grid_id="1" unit="pixels" width="240px"]
[mds3_page type="stats" grid_id="1" unit="pixels" width="260px" number_color="#2563eb" label_color="#475569"]
[mds3_page type="payment" grid_id="1"]
[mds3_page type="thank-you" grid_id="1"]
```

Million Dollar Script page roles are grid-first. `order` opens the interactive grid flow, `list` displays active advertiser placements, `manage` lists the signed-in customer's orders, and `upload`, `payment`, `confirm-order`, and `thank-you` show order-specific content when opened with a Million Dollar Script order id and order key.

## Legacy Million Dollar Script 2 Shortcodes

## Attributes

- `type` (string): One of: `grid`, `users` (legacy), `order`, `write-ad`, `confirm-order`, `payment`, `manage`, `list`, `stats`, `thank-you`, `upload`, `no-orders`. Default: `grid`.
- `id` (int): Grid/banner id. Default: `1`. For `list`, omitting `id` shows all grids.
- `align` (string): `left`, `right`, `center`. Default: `center`.
- `width` (string): CSS dimension, e.g. `100%`, `1000px`, `auto`. Default: `100%` (except Grid; see notes).
- `height` (string): CSS dimension, e.g. `auto`, `1000px`. Default: `auto` (except Grid; see notes).
- `lang` (string): Language code; default `EN` (legacy compatibility).

Notes:

- Grid dimensions should match your configured grid (see below). For blocks, you can use `{width}` and `{height}` placeholders, which resolve to the exact grid size from the database.
- The plugin will attempt to infer sensible dimensions in some cases (`Functions::maybe_set_dimensions()`), but for the Grid view use explicit sizes or the block placeholders for best alignment.

## Types

- `grid`: Public grid image and tooltip interactions.
  - Example: `[milliondollarscript id="1" type="grid" width="1000px" height="1000px" align="center"]`
  - Tip: When using the Gutenberg block, set width/height to `{width}`/`{height}` for exact sizing.

- `users` (legacy): Combined buy/ordering view. Kept for parity; prefer splitting into the newer steps below if you want more control over copy/layout.
  - Example: `[milliondollarscript id="1" type="users" width="100%" height="auto"]`

- `order`: Start an order (select blocks and begin flow).
- `write-ad`: Upload artwork and enter link/pop-up text.
- `confirm-order`: Review and confirm before payment.
  - Example (step pages):
    - `[milliondollarscript id="1" type="order"]`
    - `[milliondollarscript id="1" type="write-ad"]`
    - `[milliondollarscript id="1" type="confirm-order"]`

- `payment`: Payment step. With an order id/key, it shows the order summary and a WooCommerce or standalone checkout link when one is available.
  - Example: `[milliondollarscript type="payment"]`

- `manage`: Public-facing manage pixels page for users to update their ad.
  - Example: `[milliondollarscript id="1" type="manage" width="100%" height="auto"]`

- `list`: Advertiser list; omit `id` to show all grids.
  - Example: `[milliondollarscript type="list" width="100%" height="auto"]`

- `stats`: Compact stats box.
  - Shows sold and available inventory.
  - Unit follows Settings > Display > Stats Display Mode unless `unit="blocks"` or `unit="pixels"` is set on the shortcode or stats block.
  - Recommended: `width="240px"`
  - Example: `[milliondollarscript id="1" type="stats" width="150px" height="60px"]`

- `thank-you`: Thank-you view after payment.
- `upload`: Upload step (usually reached inside the flow).
- `no-orders`: Fallback display when no orders are present.

## Examples

Minimal grid page:

```
[milliondollarscript id="1" type="grid" width="1000px" height="1000px"]
```

List all advertisers across every grid:

```
[milliondollarscript type="list" width="100%" height="auto"]
```

Stats box for a single grid:

```
[milliondollarscript id="1" type="stats" width="150px" height="60px"]
```

Ordering as a single legacy screen:

```
[milliondollarscript id="1" type="users" width="100%" height="auto"]
```

Step-based ordering (separate pages):

```
[milliondollarscript id="1" type="order"]
[milliondollarscript id="1" type="write-ad"]
[milliondollarscript id="1" type="confirm-order"]
```

Payment handoff (with WooCommerce enabled):

```
[milliondollarscript type="payment"]
```

## Sizing Guidance

- Grid: match your grid’s exact pixel dimensions or use the block with `{width}`/`{height}` placeholders.
- List/Manage/Users/Order/Confirm/Thank-you: generally `width="100%" height="auto"` integrates best with themes.
- Stats: `150px × 60px` keeps the UI crisp.
