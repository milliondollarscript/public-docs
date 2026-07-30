---
slug: extension-development
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [developers, administrators]
published: true
tags: [developers, extensions, hooks, blocks]
---

# Extension Development

A Million Dollar Script extension is a separate WordPress plugin that adds one focused capability while using core for grids, orders, payments, API governance, setup, and shared admin navigation.

## Minimal Plugin

```php
<?php
/**
 * Plugin Name:       Million Dollar Script - Example
 * Description:       Adds an example workflow to Million Dollar Script.
 * Version:           1.0.0
 * Requires at least: 6.0
 * Requires PHP:      8.1
 * Requires Plugins:  million-dollar-script
 * Requires MDS:      3.0.0
 * Requires MDS API:  1
 * MDS Generation:    3
 * MDS Compatible:    MDS 3.0
 * Text Domain:       mds-example
 */

if (!defined('ABSPATH')) {
    exit;
}

final class MDS_Example_Plugin {
    public function boot(): void {
        add_filter('million-dollar-script/extension/onboarding/items', [$this, 'onboarding']);
        add_filter('million-dollar-script/editor/extension/blocks', [$this, 'blocks']);
        add_shortcode('mds_example', [$this, 'shortcode']);
    }

    public function onboarding(array $items): array {
        $items['mds-example'] = [
            'name' => __('Example', 'mds-example'),
            'summary' => __('Configure the example workflow.', 'mds-example'),
            'priority' => 100,
            'actions' => [[
                'label' => __('Open Example', 'mds-example'),
                'url' => admin_url('admin.php?page=mds-example'),
                'primary' => true,
            ]],
        ];

        return $items;
    }

    public function blocks(array $blocks): array {
        $blocks[] = [
            'name' => 'mds-example/content',
            'title' => __('Example', 'mds-example'),
            'description' => __('Display Example content from Million Dollar Script.', 'mds-example'),
            'icon' => 'screenoptions',
            'category' => 'widgets',
            'attributes' => [
                'title' => ['type' => 'string', 'default' => __('Example', 'mds-example')],
            ],
            'controls' => [[
                'attribute' => 'title',
                'type' => 'text',
                'label' => __('Title', 'mds-example'),
                'help' => __('Heading shown above the content.', 'mds-example'),
            ]],
            'preview' => [
                'title' => __('Example', 'mds-example'),
                'description' => __('Shows the configured Example content.', 'mds-example'),
            ],
            'render_callback' => [$this, 'render_block'],
        ];

        return $blocks;
    }

    public function render_block(array $attributes = []): string {
        return $this->shortcode([
            'title' => sanitize_text_field((string) ($attributes['title'] ?? '')),
        ]);
    }

    public function shortcode(array $attributes = []): string {
        $attributes = shortcode_atts(['title' => ''], $attributes, 'mds_example');
        return sprintf(
            '<section class="mds-example"><h2>%s</h2></section>',
            esc_html((string) $attributes['title'])
        );
    }
}

add_action('plugins_loaded', static function (): void {
    if (!class_exists(\MillionDollarScript\Core\Runtime::class) || !\MillionDollarScript\Core\Runtime::is_ready()) {
        return;
    }
    (new MDS_Example_Plugin())->boot();
}, 20);
```

## Recommended Structure

```text
mds-example/
├── mds-example.php
├── src/
│   ├── Plugin.php
│   ├── Admin/
│   ├── Domain/
│   └── Rest/
├── templates/
├── assets/
│   ├── css/
│   └── js/
├── languages/
├── tests/
├── composer.json
└── readme.txt
```

Keep request handling, persistence, rendering, and business rules separate. Load admin assets only on the extension's screens and frontend assets only when its output is present.

## Supported Core Access

Use the version-neutral facades documented in the [Developer Overview](/docs/mds-3/million-dollar-script/3.0.0/main/developer-overview). Implementation namespaces and ambiguous globals such as `MDS_VERSION` are not extension contracts.

For browser code, publish configuration with `MillionDollarScript\Extensions\Support::add_browser_config()`. Read it from `window.MillionDollarScript.extensions`; do not add standalone globals.

## Admin Navigation and Setup

Register extension actions with `million-dollar-script/extension/onboarding/items`. Core uses this information in setup, dashboard cards, and extension navigation while the plugin is active. Do not modify the WordPress admin menu DOM.

Use `million-dollar-script/dashboard/extension/cards` only when the default card needs additional actions or status. Use `million-dollar-script/admin/bar/extension/items` only when the admin-bar destination differs from the primary onboarding action.

An extension may include `recommended_pages` and `legal_documents` in its onboarding item. Legal drafts must state that they are not legal advice and should use setup placeholders supplied by core instead of hardcoded site contact details.

## Blocks

Register dynamic blocks through `million-dollar-script/editor/extension/blocks`. Provide a clear title, customer-facing description, icon, attributes, controls, preview, and server render callback. Prefer selection controls for existing records; offer a custom ID only as an explicit fallback when a large data set cannot be loaded.

The block preview should resemble the frontend or show a legible placeholder. Do not expose shortcode syntax as the visual preview.

## Payments

Monetized extensions call `MillionDollarScript\Commerce\Payments::create_checkout()` and identify their record with `source` and `source_id`. They must not call WooCommerce or another gateway directly.

```php
$checkout = \MillionDollarScript\Commerce\Payments::create_checkout([
    'source' => 'mds-example',
    'source_id' => $record_id,
    'email' => $customer_email,
    'currency' => \MillionDollarScript\Commerce\Currency::current_code(),
    'total' => 49.00,
    'items' => [[
        'name' => 'Example placement',
        'amount' => 49.00,
        'quantity' => 1,
    ]],
    'manage_url' => $private_manage_url,
]);
```

Listen to `million-dollar-script/payment/source/status` and update only records whose source matches your extension. Provider extensions register through `million-dollar-script/payment/provider/options` and `million-dollar-script/payment/providers`.

## REST API Integration

Register routes with the WordPress REST API, then describe every governed route through `million-dollar-script/api/endpoint/manifest`. Supply a stable endpoint ID, full `/million-dollar-script/v1/...` route pattern, methods, scope, minimum security level, and description. Core omits incomplete entries instead of assigning permissive defaults.

Use `million-dollar-script/api/openapi/document` to add schemas and examples to the generated OpenAPI 3.1 document.

For an endpoint whose minimum security level is `service_signature`, register each extension-owned credential through the stable public facade:

```php
use MillionDollarScript\Core\ApiAccess;
use MillionDollarScript\Core\ServiceSignatureRequest;

ApiAccess::register_service_signature_verifier(
    'your-stable-endpoint-id',
    'your.scope.write',
    $opaque_service_id,
    static function (ServiceSignatureRequest $signature, WP_REST_Request $request) {
        // Check status, expiry, revocation, scope, ownership, HMAC, nonce, and rate.
        return $verified_and_nonce_claimed
            ? true
            : ApiAccess::service_signature_error('invalid');
    },
    ['v1']
);
```

Core accepts only literal `true`. A safe `WP_Error` is normalized, exceptions and malformed returns are denied, and no registration means remote access remains unavailable. Read the verified non-administrator identity with `ApiAccess::service_identity($request)`. Extensions must own credential creation, one-time secret exchange, encryption, rotation, revocation, replay storage, and relationship checks; do not use private `V3` classes or treat a label, URL, service ID, or header as authentication.

## Packaging and Updates

- Keep the extension slug and main plugin basename stable.
- Use semantic versions without a `v` prefix.
- Update both plugin headers and readme stable tags.
- Do not bundle development dependencies or tests in production ZIPs unless they are needed at runtime.
- Paid extensions must validate entitlement through the extension server and fail closed for package downloads and private documentation.
- Declare whether the extension is compatible with MDS 3.0 so MDS 2 and MDS 3.0 catalogs remain separated.
- Run the workspace release audit before packaging. Extension packaging rejects private namespaces, pre-release browser globals, and ambiguous core constants.

## Verification

Test activation, deactivation, uninstall policy, capability failures, nonce failures, multisite behavior where supported, mobile admin layouts, keyboard navigation, dark and light admin themes, API rate limits, payment cancellation, and plugin updates. Run PHP syntax checks and the extension's automated test suite before packaging.
