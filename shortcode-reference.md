# Shortcode Reference

The plugin provides a single shortcode, `[milliondollarscript]`, which renders different views depending on the `type` parameter. Most page layouts can also be added via the “Million Dollar Script” Gutenberg block, which sets the same parameters under the hood.

## Attributes

- `type` (string) — One of: `grid`, `users` (legacy), `order`, `write-ad`, `confirm-order`, `payment`, `manage`, `list`, `stats`, `thank-you`, `upload`, `no-orders`. Default: `grid`.
- `id` (int) — Grid/banner id. Default: `1`. For `list`, omitting `id` shows all grids.
- `align` (string) — `left`, `right`, `center`. Default: `center`.
- `width` (string) — CSS dimension, e.g. `100%`, `1000px`, `auto`. Default: `100%` (except Grid; see notes).
- `height` (string) — CSS dimension, e.g. `auto`, `1000px`. Default: `auto` (except Grid; see notes).
- `lang` (string) — Language code; default `EN` (legacy compatibility).

Notes:

- Grid dimensions should match your configured grid (see below). For blocks, you can use `{width}` and `{height}` placeholders, which resolve to the exact grid size from the database.
- The plugin will attempt to infer sensible dimensions in some cases (`Functions::maybe_set_dimensions()`), but for the Grid view use explicit sizes or the block placeholders for best alignment.

## Types

- `grid` — Public grid image and tooltip interactions.
  - Example: `[milliondollarscript id="1" type="grid" width="1000px" height="1000px" align="center"]`
  - Tip: When using the Gutenberg block, set width/height to `{width}`/`{height}` for exact sizing.

- `users` (legacy) — Combined buy/ordering view. Kept for parity; prefer splitting into the newer steps below if you want more control over copy/layout.
  - Example: `[milliondollarscript id="1" type="users" width="100%" height="auto"]`

- `order` — Start an order (select blocks and begin flow).
- `write-ad` — Upload artwork and enter link/pop-up text.
- `confirm-order` — Review and confirm before payment.
  - Example (step pages):
    - `[milliondollarscript id="1" type="order"]`
    - `[milliondollarscript id="1" type="write-ad"]`
    - `[milliondollarscript id="1" type="confirm-order"]`

- `payment` — Payment step; when WooCommerce is enabled and conditions are met, redirects to the Woo checkout.
  - Example: `[milliondollarscript type="payment"]`

- `manage` — Public-facing manage pixels page for users to update their ad.
  - Example: `[milliondollarscript id="1" type="manage" width="100%" height="auto"]`

- `list` — Advertiser list; omit `id` to show all grids.
  - Example: `[milliondollarscript type="list" width="100%" height="auto"]`

- `stats` — Compact stats box.
  - Recommended: `width="150px" height="60px"`
  - Example: `[milliondollarscript id="1" type="stats" width="150px" height="60px"]`

- `thank-you` — Thank-you view after payment.
- `upload` — Upload step (usually reached inside the flow).
- `no-orders` — Fallback display when no orders are present.

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

