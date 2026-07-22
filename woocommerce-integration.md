---
slug: woocommerce-integration
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [site-owners, administrators]
published: true
---

# WooCommerce Integration

WooCommerce support is provided by the **Million Dollar Script WooCommerce Checkout** extension. The core plugin exposes the payments API; the WooCommerce extension registers WooCommerce as a payment provider. For non-WooCommerce manual or external checkout URLs, see [Checkout And Payments](/docs/mds-3/million-dollar-script/3.0.0/main/mds3-checkout-and-payments).

## Enable integration

- Install and configure WooCommerce following their docs (gateways, taxes, emails, checkout page).
- Install and activate **Million Dollar Script WooCommerce Checkout**.
- In **Million Dollar Script → Setup**, choose **WooCommerce** as the payment provider.
- Result: Million Dollar Script creates a linked WooCommerce order and redirects to WooCommerce checkout after artwork upload.
- If WooCommerce is disabled later, the provider is not registered until WooCommerce is active again.

## Behavior & options

- Refund handling hooks into WooCommerce refunds.
- WooCommerce `processing`, `completed`, and `payment_complete` events mark the linked Million Dollar Script order paid.
- WooCommerce `cancelled`, `failed`, and `refunded` events cancel the linked Million Dollar Script order and release its blocks.
- The extension can coordinate login/register with Woo’s My Account pages. See **Million Dollar Script → Settings → WooCommerce Checkout** for the WooCommerce Login Redirect URL.
- WooCommerce account orders include a **Manage** action for linked Million Dollar Script orders when the signed-in customer owns the order. Administrators can also see the action while managing the site.

## Shortcode/Page examples

- Payment handoff: `[milliondollarscript type="payment"]`
- Thank‑you page (optional): `[milliondollarscript type="thank-you"]`

## Troubleshooting

For WooCommerce-related issues, see [Troubleshooting: WooCommerce Issues](/docs/mds-3/million-dollar-script/3.0.0/main/troubleshooting#woocommerce-issues).
