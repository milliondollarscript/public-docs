# Dynamic CSS & Theme Modes

MDS generates a dynamic CSS file based on Options, enabling light/dark themes, accessible colors, and consistent UI styling.

## How it works

- Options control theme variables (colors, button styles, etc.).
- Saving Options regenerates the CSS file under `wp-content/uploads/milliondollarscript/dynamic-styles.css`.
- The CSS is scoped to the `.mds-container` and WordPress login page to avoid global leaks.

## Regenerate manually (optional)

- WP‑CLI: `wp eval '\\MillionDollarScript\\Classes\\Web\\Styles::save_dynamic_css_file(); echo "OK\n";'`

## Tips

- Prefer theme options over custom CSS so updates remain compatible.
- Link colors and badges are accessible by default; your theme can override within the `.mds-container` scope.
- If styles appear off after an update, save Options once to recompute the CSS.

