---
slug: backups-and-recovery
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [site-owners, administrators]
published: true
tags: [backups, recovery, migration, settings, operations]
---

# Backups And Recovery

Create a complete database and `wp-content/uploads` backup before migration, bulk changes, major updates, or payment-provider changes. A settings export is useful for configuration recovery, but it does not replace a full site backup.

## Settings Export And Import

Open **Million Dollar Script -> Settings -> Import / Export** to download the current settings as JSON.

When importing settings:

1. Choose a Million Dollar Script settings JSON file no larger than 1 MB.
2. Review the preview of changed, unknown, and rejected values.
3. Apply the import only after the preview matches the intended site configuration.
4. Recheck page assignments, payment settings, currency, email messages, upload limits, and extension connections.

The importer validates known settings and rejects unsupported values. It does not import grids, orders, media, extension records, WordPress pages, users, or payment-provider data.

## Migration Recovery

Run the Million Dollar Script 2 migration dry run before importing. Review source-table counts and warnings, then take a fresh backup. The migration records progress so an interrupted local request can resume, but a completed data import is not a substitute for a reversible backup.

To fully return to the pre-migration state, restore the database and uploads backup created immediately before migration. Deactivating Million Dollar Script does not remove imported data.

## Order And Reservation Recovery

- Customer form details can be restored from the same browser for up to seven days.
- Successfully uploaded draft images can be restored for up to three days while their order remains valid.
- Expired unpaid reservations are released by scheduled cleanup.
- Paid orders, active placements, and published media should be recovered from the database and uploads backup, not browser storage.

## Operational Checklist

- Schedule automated database and uploads backups outside the public web root.
- Test restoration on a staging copy instead of assuming a backup is usable.
- Keep WordPress cron or a real server cron running so reservations, reminders, and stale media are processed.
- Record the active Million Dollar Script, WordPress, PHP, database, WooCommerce, and extension versions before troubleshooting.
- Remove API keys, license keys, tester keys, order keys, manage tokens, email addresses, and payment identifiers from logs shared with support.

See [Troubleshooting](/docs/mds-3/million-dollar-script/3.0.0/main/troubleshooting) for routing, checkout, update, rendering, and extension diagnostics.
