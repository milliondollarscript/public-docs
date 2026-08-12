---
slug: advertisers-validation-and-clicks
navigation_title: Advertisers, validation, and clicks
product_generation: mds-2
package_slug: million-dollar-script
package_type: core
package_version: "2.6"
channel: main
access: public
audience: [site-owners, administrators]
published: true
tags: [mds2, advertisers, validation, click-tracking]
---

# Advertisers, Validation, and Click Tracking

The current MDS 2 WordPress plugin uses WordPress accounts and plugin-managed order data. Older standalone documentation may describe a separate advertiser account table and should not be followed on a WordPress installation.

## Field length limits

Names, email addresses, URLs, and advertisement copy are validated according to the current WordPress and Million Dollar Script forms. The exact limit depends on the field rather than one global character count.

Keep URLs within normal browser and server limits, use the dedicated description fields for longer text, and rely on the counter or validation message shown by the current form. Do not alter database column sizes without first confirming the field definition in the installed plugin version.

## Delete an advertiser

Before deleting an advertiser, review their orders and active placements. Cancelling or removing an order is different from deleting its WordPress user account.

Use the Million Dollar Script order and advertiser screens for campaign records. Use **Users** in WordPress only when the account itself should be removed. When WordPress asks what to do with the user's content, reassign any content that must be preserved.

Create a database backup before a bulk cleanup. Do not delete records directly from the database because orders, placements, users, and payment records can reference one another.

## Account and URL validation

WordPress manages account identity, passwords, and any site-level email verification workflow. Million Dollar Script validates order and advertisement data needed for its own workflow. A site can also require email confirmation through its membership, commerce, or security plugin.

URL validation checks that a submitted destination is usable and permitted by the configured rules. It does not guarantee that the destination will remain safe or available. Administrators should still review advertisements and use an appropriate moderation policy.

## Click tracking

When click tracking is enabled, Million Dollar Script records visits through its tracking route so the administrator and advertiser can review campaign activity. These counts are operational statistics, not a replacement for a full analytics platform.

Before enabling tracking, review your privacy notice, retention needs, caching rules, and consent requirements. Exclude tracking routes from full-page caching. If a CDN or security plugin blocks the redirect, allow the exact Million Dollar Script tracking route rather than disabling site-wide protection.

## Schema errors from older releases

An error such as `Field 'Aboutme' doesn't have a default value` usually indicates an incomplete or mismatched legacy database schema. Update to the current supported MDS 2 WordPress release and let its upgrade routine run. Restore from backup or contact support if the upgrade cannot complete.

Do not switch MySQL to an obsolete compatibility mode or weaken strict SQL settings to hide the error. That can conceal damaged or incomplete data.

## Million Dollar Script 3.0

MDS 3.0 has separate order, placement, API, and extension workflows. Use its own admin screens and documentation. Do not delete MDS 3.0 rows with MDS 2 maintenance instructions.
