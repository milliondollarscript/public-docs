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

- Verify WooCommerce’s checkout page and endpoints are configured and published.
- If redirects don’t occur, ensure you are logged in and an order id is present as required by the flow.
- Clear caches after changing Woo settings, then retest.

