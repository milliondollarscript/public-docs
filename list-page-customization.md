---
slug: list-page-customization
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [developers, administrators]
published: true
tags: [developers, advertiser-list, hooks]
---

# Advertiser List Customization

The advertiser list is available through the Page Flow block set to Advertiser List and the `[mds3_page type="list"]` shortcode. With a `grid_id`, it shows one active grid. Without a grid id, it provides bounded server-side search and pagination across published advertisers on every active grid, and labels each result with its grid.

Core search covers advertiser title or alt text, website URL, popup description, grid title, and grid slug. Only public placement fields reach the list-value hooks; customer email, order keys, manage tokens, and private order metadata are not included.

## Add a Column

```php
add_filter('million-dollar-script/advertiser/list/columns', function (array $columns): array {
    $columns['campaign'] = __('Campaign', 'mds-example');
    return $columns;
});
```

## Supply Values

```php
add_filter('million-dollar-script/advertiser/list/item/values', function (array $values, array $placement, array $columns): array {
    $values['campaign'] = sanitize_text_field((string) ($placement['grid_title'] ?? ''));
    return $values;
}, 10, 3);
```

Return plain public values or explicitly sanitized image markup supported by the list renderer. Never look up and expose customer email, order keys, manage tokens, private metadata, or unpublished placement data.

## Make An Extension Column Searchable

When a search term can match data stored by your extension, return the matching public placement IDs. Query only your extension's public data and keep the result bounded. Million Dollar Script still enforces active placement and active grid visibility.

```php
add_filter('million-dollar-script/advertiser/list/search/placement/ids', function (array $ids, string $search, array $context): array {
    if ('' === trim($search)) {
        return $ids;
    }

    $campaign_ids = mds_example_public_campaign_placement_ids($search, $context['grid_id'] ?? 0, 1000);

    return array_values(array_unique(array_merge($ids, array_map('absint', $campaign_ids))));
}, 10, 3);
```

Use `million-dollar-script/advertiser/list/per/page` to adjust the default page size of 24. Core clamps the value to 1-100 so the public page cannot load every placement into memory.

## Add a Layout

Use `million-dollar-script/advertiser/list/layouts` to register a named layout that can appear in block and shortcode controls. Keep responsive behavior inside the extension stylesheet, load it only when the list is present, and verify narrow touch layouts and keyboard navigation.

MDS 2 used `mds_list_*` hooks and the `milliondollarscript` shortcode. Those are documented under the MDS 2 version and should not be used for new MDS 3.0 extensions.
