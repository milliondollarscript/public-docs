---
slug: logs-and-cron
navigation_title: Logs and scheduled tasks
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [site-owners, administrators]
published: true
tags: [cron, scheduled-tasks, troubleshooting, logs]
---

# Scheduled Tasks and Logs

Million Dollar Script uses WordPress scheduling for order cleanup. Extensions may register their own retention, expiry, reporting, or synchronization jobs.

## Core Order Cleanup

Core schedules the `mds3_order_cleanup` hook hourly. The job expires stale reservations and releases inventory according to current order rules. WP-Cron must be able to run for abandoned reservations to clear without administrator activity.

## Verify WP-Cron

1. Open **Tools > Site Health** and resolve loopback or scheduled-event errors.
2. Confirm `DISABLE_WP_CRON` is not enabled unless the host runs `wp-cron.php` from a real system scheduler.
3. With WP-CLI, run `wp cron event list` and locate `mds3_order_cleanup`.
4. On staging, run the due event and confirm order and block state rather than repeatedly forcing it on production.

For low-traffic sites, configure a system cron request to `wp-cron.php` at a suitable interval. Protect access according to the host's WordPress guidance.

## Logs

Million Dollar Script does not expose private customer data or complete credentials in its API audit log. For PHP failures, enable `WP_DEBUG_LOG` on staging and keep `WP_DEBUG_DISPLAY` disabled. WooCommerce payment events and scheduled actions remain in WooCommerce's logging tools when that provider is active.

Million Dollar Script sends email through `wp_mail()` and does not maintain an email log. Use a reputable SMTP or mail-logging plugin when delivery diagnostics are required.

Remove API keys, license keys, tester keys, order keys, manage tokens, customer email addresses, and raw request bodies before sharing logs.

## Extension Jobs

Extensions should use a unique hook, schedule idempotently, clear it on deactivation, and retain data unless uninstall explicitly authorizes deletion. A job must check capabilities or service signatures at the request boundary, lock overlapping runs, process bounded batches, and record only operational details needed for diagnosis.
