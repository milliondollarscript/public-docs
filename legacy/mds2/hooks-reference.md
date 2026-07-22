---
slug: hooks-reference
product_generation: mds-2
package_slug: million-dollar-script
package_type: core
package_version: "2.6"
channel: main
access: public
audience: [developers, administrators]
published: true
tags: [mds2, legacy]
---

# Hooks Reference

MDS provides actions and filters for extending functionality. This reference covers the most commonly used hooks for extension developers.

## Menu Hooks

MDS uses a structured `Menu_Registry` system for admin menu integration. Extensions register menu items as structured data rather than echoing raw HTML.

### mds_register_dashboard_menu

Register menu items in the MDS admin dashboard. This is the primary hook for adding navigation entries.

**Callback signature:** `function (string $registry_class): void`

The callback receives the fully-qualified `Menu_Registry` class name, which you call statically to register items.

```php
add_action('mds_register_dashboard_menu', function (string $registry_class): void {
    $registry_class::register([
        'slug'     => 'my-extension-settings',
        'title'    => __('My Extension', 'my-extension'),
        'url'      => admin_url('admin.php?page=my-extension-settings'),
        'parent'   => 'mds-extensions',
        'position' => 10,
    ]);
});
```

#### Parameters for `Menu_Registry::register()`

| Parameter  | Type   | Required | Description |
|-----------|--------|----------|-------------|
| `slug`     | string | Yes      | Unique identifier for the menu item |
| `title`    | string | Yes      | Display label |
| `url`      | string | Yes      | Full admin URL |
| `parent`   | string | No       | Parent menu slug (see below) |
| `position` | int    | No       | Sort order within the parent group |

#### Available parent slugs

| Parent Slug | Section | Contains |
|------------|---------|----------|
| `pixel-management` | Grid Management | Grid, Packages, Price Zones, Backgrounds, Not for Sale, Approve |
| `orders` | Orders | All Orders, Process Pixels, etc. |
| `reports` | Reports | Click Reports, Top Customers, Transaction Log |
| `system` | System | Options, Logs, Emails, License, System Info, Changelog |
| `mds-extensions` | Extensions | Extension hub (recommended for extensions) |

#### Class method example

```php
add_action('mds_register_dashboard_menu', [$this, 'register_dashboard_menu_items']);

public function register_dashboard_menu_items(string $registry_class): void {
    $registry_class::register([
        'slug'     => 'mds-translation-translations',
        'title'    => __('Translations', 'mds-translation'),
        'url'      => admin_url('admin.php?page=mds-translation-translations'),
        'parent'   => 'mds-extensions',
        'position' => 1,
    ]);
}
```

For most extensions, use `'parent' => 'mds-extensions'` so your item appears under the Extensions dropdown.

## Submenu Visibility

### mds_extensions_visible_submenus

Show additional submenus that are hidden by default. Some MDS submenus are hidden with CSS but remain accessible via direct URL.

```php
add_filter('mds_extensions_visible_submenus', function (array $slugs) {
    $slugs[] = 'my-custom-page';
    $slugs[] = 'another-page';
    return $slugs;
});
```

## List Page Hooks

The advertiser list (`/milliondollarscript/list/` route and `[milliondollarscript type="list"]` shortcode) exposes filters for customization.

### mds_list_columns

Add, remove, or reorder columns in the advertiser list.

```php
add_filter('mds_list_columns', function (array $columns) {
    $columns[] = [
        'key'               => 'launch_date',
        'label'             => __('Launch Date', 'my-extension'),
        'render_callback'   => function (array $row_context) {
            $timestamp = strtotime(carbon_get_post_meta($row_context['ad_id'], MDS_PREFIX . 'launch_date'));
            if (!$timestamp) {
                return __('Not set', 'my-extension');
            }
            return esc_html(date_i18n(get_option('date_format'), $timestamp));
        },
        'cell_attributes'    => ['class' => ['list-cell', 'list-cell--launch-date']],
        'heading_attributes' => ['class' => ['list-heading', 'list-heading--launch-date']],
        'grid_track'         => 'minmax(0, 1fr)', // Optional: CSS Grid track size
    ];
    return $columns;
});
```

### mds_list_cell_{column_key}

Modify a specific cell after rendering. The filter name includes the column key.

```php
add_filter('mds_list_cell_launch_date', function (array $cell, array $column, array $row_context) {
    $timestamp = strtotime(carbon_get_post_meta($row_context['ad_id'], MDS_PREFIX . 'launch_date'));
    if ($timestamp && $timestamp > time()) {
        $cell['content'] .= ' <span class="mds-badge mds-badge--scheduled">'
            . esc_html__('Scheduled', 'my-extension')
            . '</span>';
    }
    return $cell;
}, 10, 3);
```

### Other List Hooks

| Hook | Purpose |
|------|---------|
| `mds_list_container_attributes` | Modify wrapper div attributes |
| `mds_list_wrapper_attributes` | Modify inner wrapper attributes |
| `mds_list_orders_sql` | Customize the SQL query for fetching records |
| `mds_list_rows` | Filter the rows after fetching |
| `mds_list_row_context` | Alter the data passed to each row |
| `mds_list_row_cells` | Modify all cells in a row |
| `mds_list_heading_cell` | Modify a heading cell |
| `mds_list_heading_cells` | Modify all heading cells |
| `mds_list_grid_tracks` | Control CSS Grid column track sizes |
| `mds_list_grid_template` | Replace the final grid-template-columns string |
| `mds_list_default_grid_tracks` | Override default track sizes |
| `mds_list_fallback_grid_track` | Change the catch-all track for extra columns |
| `mds_list_banner_markup` | Customize banner row markup |
| `mds_list_link_attributes` | Modify advertiser link attributes |
| `mds_list_link_text` | Modify advertiser link text |

See [List Page Customization](/docs/mds-2/million-dollar-script/2.6/main/list-page-customization) for detailed examples.

## Form Hooks

### mds_form_fields

Add custom fields to forms using Carbon Fields.

```php
use Carbon_Fields\Field\Field;

add_filter('mds_form_fields', function (array $fields, string $prefix) {
    $fields[] = Field::make('date', $prefix . 'launch_date', __('Launch Date', 'my-extension'))
        ->set_help_text(__('When this campaign goes live.', 'my-extension'))
        ->set_storage_format('Y-m-d');
    return $fields;
}, 10, 2);
```

The `$prefix` parameter (typically `_mds_`) ensures consistent meta key naming.

## Best Practices

1. **Always check capabilities** before rendering admin content
   ```php
   if (!current_user_can('manage_options')) {
       return;
   }
   ```

2. **Use proper escaping** for all output
   - `esc_url()` for URLs
   - `esc_html()` for text content
   - `esc_attr()` for HTML attributes

3. **Keep callbacks lightweight** - Avoid heavy database queries or API calls in hook callbacks

4. **Use meaningful keys** for columns and fields that won't conflict with core or other extensions

5. **Document your hooks** so other developers can extend your extension

6. **Test with multiple grids** - Ensure your customizations work correctly when multiple grids appear on the same page

## Menu CSS and Behavior

- Dropdowns open with hover intent (approximately 100ms open, 400ms close delay) and keyboard focus
- The menu supports wrapping to multiple lines on narrow screens
- Submenu items are left-aligned with comfortable line-height and padding
- Hidden legacy submenus use the CSS class `mds-hidden-submenu`
