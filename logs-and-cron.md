# Logs & Cron

MDS ships with optional logging and scheduled tasks for housekeeping.

## Logging

- Enable logging under Options → System.
- Log files live under `wp-content/uploads/milliondollarscript/` with date‑stamped filenames.
- Find/tail the current log via WP‑CLI:
  - `LOG_FILE=$(wp eval 'echo \\MillionDollarScript\\Classes\\System\\Logs::get_log_file_path();')`
  - `test -n "$LOG_FILE" && tail -f "$LOG_FILE"`

## Cron tasks

- Minute/hourly/daily jobs handle order expiration, temp file cleanup, and pruning old logs.
- Run jobs manually via WP‑CLI when debugging:
  - `wp cron event list | grep milliondollarscript`
  - `wp cron event run milliondollarscript_cron_minute`
  - `wp cron event run milliondollarscript_clean_temp_files`
  - `wp cron event run milliondollarscript_clean_old_logs`

## Notes

- Logging adds useful context while setting up payment or WooCommerce flows—remember to disable it in production if you prefer quieter disks.
- Daily cleanup removes logs older than 30 days by default.

