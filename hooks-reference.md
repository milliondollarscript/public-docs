# Hooks Reference

Million Dollar Script provides actions and filters for extending functionality. This reference covers the most commonly used hooks for extension developers.

## Menu Hooks

Million Dollar Script uses a structured `Menu_Registry` system for admin menu integration. Extensions register menu items as structured data rather than echoing raw HTML.

### mds_register_dashboard_menu

Register menu items in the Million Dollar Script admin dashboard. This is the primary hook for adding navigation entries.

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

Show additional submenus that are hidden by default. Some Million Dollar Script submenus are hidden with CSS but remain accessible via direct URL.

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

See [List Page Customization](/docs/list-page-customization) for detailed examples.

## Million Dollar Script Payment Provider Hooks

Million Dollar Script exposes payment provider hooks for custom gateway extensions.

### mds3_payment_provider_options

Add a provider to the Setup wizard selector.

```php
add_filter('mds3_payment_provider_options', function (array $options): array {
    $options['my-gateway'] = __('My Gateway', 'my-extension');
    return $options;
});
```

### mds3_payment_providers

Register the provider runtime callbacks.

```php
add_filter('mds3_payment_providers', function (array $providers): array {
    $providers['my-gateway'] = [
        'id' => 'my-gateway',
        'label' => __('My Gateway', 'my-extension'),
        'ready' => true,
        'create_checkout' => 'my_gateway_create_checkout',
        'complete_source_order' => 'my_gateway_complete_source_order',
    ];

    return $providers;
});
```

Gateway callbacks should call `MDS3\Commerce\Payments::mark_source_paid()` or `MDS3\Commerce\Payments::mark_source_cancelled()` so blocks, placements, and extension-owned records stay synchronized.

### mds3_payments_pre_checkout_payload

Filter the normalized checkout payload before provider-specific callbacks run.

**Callback signature:** `function (array $payload, array $transaction, array $provider): array`

Use this for adding extension metadata that the active payment provider should see.

### mds3_payments_checkout_payload

Filter the final checkout payload returned by `MDS3\Commerce\Payments::create_checkout()`.

**Callback signature:** `function (array $payload, array $transaction, array $provider): array`

Use this for read-only presentation changes such as custom labels or extension-owned manage URLs.

### mds3_payment_source_paid

Runs when the core payments API marks an extension-owned source record as paid.

**Callback signature:** `function (string $source, int $source_id, array $context): void`

### mds3_payment_source_cancelled

Runs when the core payments API marks an extension-owned source record as cancelled.

**Callback signature:** `function (string $source, int $source_id, array $context): void`

### mds3_payment_source_status

Runs for every source payment status transition.

**Callback signature:** `function (string $source, int $source_id, string $status, array $context): void`

## API Governance Hooks

Million Dollar Script exposes endpoint discovery, policy management, scoped API keys, key rotation, and an OpenAPI contract for core and extension routes.

### mds3_api_endpoint_manifest

Register extension REST endpoints so they appear in **API Access**, discovery, and OpenAPI output.

**Callback signature:** `function (array $endpoints): array`

Each endpoint should provide:

| Key | Type | Description |
|-----|------|-------------|
| `id` | string | Stable endpoint ID, unique across core and extensions |
| `route` | string | Full REST route pattern, such as `/mds/v3/my-extension/items` |
| `methods` | array | HTTP methods governed by this policy |
| `scope` | string | Required API key scope |
| `minimum_security_level` | string | Minimum policy level core should enforce |
| `description` | string | Human-readable description for admins and OpenAPI |

Supported policy levels are `public_read`, `public_write_nonce`, `api_key_read`, `api_key_write`, `wp_capability`, and `disabled`. Internal stronger levels may be listed for future signed-token and service-to-service flows; treat them as administrator-only unless your extension implements and documents the verifier.

### mds3_api_openapi_document

Customize the generated OpenAPI 3.1 document.

**Callback signature:** `function (array $document, MDS3\Rest\ApiGovernance $governance): array`

Use this to add extension schemas, examples, or tags after your routes have been included through `mds3_api_endpoint_manifest`.

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
