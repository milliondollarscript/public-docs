# MDS3 Checkout And Payments

MDS3 supports two core checkout modes: WooCommerce checkout and standalone/manual checkout. Existing MDS2 sites can keep using the old Checkout URL pattern while they move to MDS3.

## WooCommerce Checkout

Use WooCommerce when you want card payments, gateway webhooks, refunds, order emails, taxes, or WooCommerce account pages.

1. Install and configure WooCommerce.
2. Configure your WooCommerce payment gateway.
3. In MDS Options, enable Prefer WooCommerce Checkout.
4. Test a small grid purchase with the gateway sandbox mode.

When WooCommerce checkout is enabled, MDS3 creates a linked WooCommerce order during reservation and sends the customer to the WooCommerce payment URL after artwork upload. WooCommerce `processing`, `completed`, and `payment_complete` events mark the linked MDS3 order paid, mark blocks sold, and publish the placement. WooCommerce `cancelled`, `failed`, and `refunded` events cancel the linked MDS3 order and release the blocks.

## Standalone Or Manual Checkout

Use standalone checkout when you are collecting payment offline or through an external payment page that is not WooCommerce.

In MDS Options, leave Prefer WooCommerce Checkout disabled and set Checkout URL only if you have an external/manual payment page.

If Checkout URL is set, MDS3 redirects customers there after they reserve blocks and upload artwork. If Checkout URL is empty, MDS3 sends customers to the thank-you/order-summary page after upload. The order remains pending payment unless Auto-complete Manual Payments is enabled.

## MDS2 Checkout URL Placeholders

MDS3 keeps the MDS2 placeholder format for existing external checkout links:

```text
%AMOUNT%
%CURRENCY%
%QUANTITY%
%ORDERID
%ORDERID%
%USERID%
%GRID%
%PIXELID%
```

Example:

```text
https://payments.example.com/mds?amount=%AMOUNT%&currency=%CURRENCY%&order=%ORDERID&grid=%GRID%&pixel=%PIXELID%
```

When the URL does not contain placeholders, MDS3 appends order context automatically:

```text
mds3_order_id
mds3_order_key
amount
currency
quantity
grid_id
block_id
return_url
```

## Manual Auto-complete

Auto-complete Manual Payments is for offline/manual checkout flows where no gateway webhook will confirm payment.

- Disabled: after upload, the order moves to pending payment and an admin must mark it paid.
- Enabled: after upload, the order is marked paid, selected blocks become sold, and the placement becomes active.

Leave this disabled when WooCommerce or a real gateway is responsible for payment confirmation.

## Custom Gateway Extensions

Custom checkout integrations can use the `mds3_commerce_provider` and `mds3_checkout_payload` filters to provide a provider name and payment URL. Gateway callbacks should mark MDS3 orders paid, failed, cancelled, or refunded through the MDS3 order update flow so block and placement status stays synchronized.
