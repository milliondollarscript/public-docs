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

The advertiser list is available through the List block and `[mds_list]` shortcode. MDS 3.0 extensions can add safe columns, values, and layouts without replacing the list template.

## Add a Column

```php
add_filter('million-dollar-script/advertiser/list/columns', function (array $columns): array {
    $columns['campaign'] = [
        'label' => __('Campaign', 'mds-example'),
        'priority' => 40,
    ];
    return $columns;
});
```

## Supply Values

```php
add_filter('million-dollar-script/advertiser/list/item/values', function (array $values, array $placement, array $context): array {
    $values['campaign'] = sanitize_text_field(
        (string) ($placement['metadata']['campaign'] ?? '')
    );
    return $values;
}, 10, 3);
```

Return plain values or explicitly sanitized markup supported by the list renderer. Never expose customer email, order keys, manage tokens, private metadata, or unpublished placement data.

## Add a Layout

Use `million-dollar-script/advertiser/list/layouts` to register a named layout that can appear in block and shortcode controls. Keep responsive behavior inside the extension stylesheet, load it only when the list is present, and verify narrow touch layouts and keyboard navigation.

MDS 2 used `mds_list_*` hooks and the `milliondollarscript` shortcode. Those are documented under the MDS 2 version and should not be used for new MDS 3.0 extensions.
