# Million Dollar Script Checkout And Payments

Million Dollar Script routes checkout through the core payments API. The core plugin owns orders, reservations, upload state, blocks, placements, and payment status synchronization. Payment systems are provider extensions.

## Payment Providers

Choose the provider in **Million Dollar Script -> Setup -> Payment Provider**.

- **Standalone/manual checkout** is built in.
- **Million Dollar Script WooCommerce Checkout** adds WooCommerce as a provider when both the extension and WooCommerce are active.
- Future providers such as EDD or direct gateways can register the same API.

Extensions such as SponsorBoard should not call WooCommerce, EDD, or Stripe directly. They should create their own local records, then call the Million Dollar Script payments API to start checkout. Provider extensions handle store-specific orders and gateway callbacks.

## WooCommerce Provider

Use the WooCommerce provider when you want card payments, gateway webhooks, refunds, order emails, taxes, or WooCommerce account pages.

1. Install and configure WooCommerce.
2. Configure your WooCommerce payment gateway.
3. Install and activate **Million Dollar Script WooCommerce Checkout**.
4. In **Million Dollar Script -> Setup**, choose **WooCommerce** as the payment provider.
5. Test a small grid purchase with the gateway sandbox mode.

When WooCommerce is selected, the provider extension creates a linked WooCommerce order and sends the customer to the WooCommerce payment URL after artwork upload. WooCommerce `processing`, `completed`, and `payment_complete` events mark the linked Million Dollar Script source paid. WooCommerce `cancelled`, `failed`, and `refunded` events cancel the linked source. WooCommerce account orders show a **Manage** action only to the order owner or an administrator.

## Standalone Or Manual Checkout

Use standalone checkout when you are collecting payment offline or through an external payment page that is not handled by a provider extension.

In Setup, choose **Standalone/manual checkout** and set Checkout URL only if you have an external/manual payment page.

If Checkout URL is set, Million Dollar Script redirects customers there after they reserve blocks and upload artwork. If Checkout URL is empty, Million Dollar Script sends customers to the thank-you/order-summary page after upload. The order remains pending payment unless Auto-complete Manual Payments is enabled.

## Million Dollar Script 2 Checkout URL Placeholders

Million Dollar Script keeps the Million Dollar Script 2 placeholder format for existing external checkout links:

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

When the URL does not contain placeholders, Million Dollar Script appends order context automatically:

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

Leave this disabled when a provider extension or real gateway is responsible for payment confirmation.

## Custom Gateway Extensions

Payment extensions register through:

- `mds3_payment_provider_options`
- `mds3_payment_providers`

A provider entry can expose:

- `id`
- `label`
- `ready`
- `create_checkout`
- `complete_source_order`
- `locks_currency`
- `currency_code`
- `currency_symbol`

Gateway callbacks should call the core payment status API so blocks, placements, and extension-owned records stay synchronized.
