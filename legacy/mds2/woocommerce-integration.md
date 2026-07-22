---
slug: woocommerce-integration
product_generation: mds-2
package_slug: million-dollar-script
package_type: core
package_version: "2.6"
channel: main
access: public
audience: [site-owners, administrators]
published: true
tags: [mds2, legacy]
---

# WooCommerce Integration

MDS integrates with WooCommerce for checkout, refunds, and account/login coordination.

## Enable integration

- Install and configure WooCommerce following their docs (gateways, taxes, emails, checkout page).
- In MDS Options → WooCommerce, enable the integration.
- Result: the `payment` step redirects to WooCommerce checkout when appropriate.

## Behavior & options

- Refund handling hooks into WooCommerce refunds.
- MDS can coordinate login/register with Woo’s My Account pages. See Options → Login (WooCommerce Login Redirect).
- On MDS routes, the plugin disables Woo’s “empty checkout” redirect to avoid hijacking grid flows.

## Shortcode/Page examples

- Payment handoff: `[milliondollarscript type="payment"]`
- Thank‑you page (optional): `[milliondollarscript type="thank-you"]`

## Troubleshooting

For WooCommerce-related issues, see [Troubleshooting: WooCommerce Issues](/docs/mds-2/million-dollar-script/2.6/main/troubleshooting#woocommerce-issues).

