---
slug: order-emails-and-renewals
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [site-owners, administrators]
published: true
tags: [orders, email, renewals, wp-mail]
---

# Order Emails And Renewal Notices

Million Dollar Script sends order notifications through WordPress `wp_mail()`. Open **Million Dollar Script -> Settings -> Order Emails** to choose which customer and administrator messages are enabled and to edit their subjects and content.

## Notification Events

Separate settings are available for:

- Payment requested when an order enters pending payment.
- Order paid when a new order is paid and active.
- Renewal paid when a renewal payment completes.
- Order expired when a paid placement expires or an unpaid reservation is released.
- Order denied when an administrator or integration denies an order.
- Placement published for an administrator notification after an active placement is saved.
- Renewal reminders before a paid placement reaches its expiration date.

Customer messages require a valid email address on the order. Administrator messages use the Administration Email Address under **Settings -> General**.

## Editing Messages

Message editors accept formatted content and resize to the saved message. Use the placeholder buttons beside an editor to insert supported values. Common placeholders include:

- `%SITE_NAME%` and `%SITE_URL%`
- `%ORDER_ID%`, `%GRID_NAME%`, `%STATUS%`, and `%PRICE%`
- `%CUSTOMER_EMAIL%`, `%FIRST_NAME%`, and `%LAST_NAME%`
- `%MANAGE_URL%` and `%VIEW_URL%`
- `%DAYS_LEFT%`, `%EXPIRES_AT%`, and `%REASON%`

Only the values available for an event are populated. Send a complete test order through the selected payment flow before relying on a customized template.

## Renewal Reminders

Renewal reminders can be sent to the customer, the administrator, or both. The three reminder-day settings define how many days before expiration each notice becomes due. WordPress cron must be working for scheduled reminders and expiration processing to run reliably.

Renewal payment handling belongs to the active payment provider. WooCommerce Checkout creates a payable WooCommerce renewal order; another provider can implement the same core payment contract.

## Delivery Troubleshooting

Million Dollar Script does not keep a separate mail log. If a message is not delivered:

1. Confirm the corresponding customer or administrator toggle is enabled.
2. Confirm the order has the expected status and customer email address.
3. Check **Tools -> Site Health** for cron or loopback failures.
4. Use a reputable SMTP or mail-logging plugin to test WordPress mail delivery.
5. Remember that WooCommerce may send its own transactional messages in addition to Million Dollar Script order notices.

Do not include passwords, API keys, license keys, order keys, or other secrets in email templates.
