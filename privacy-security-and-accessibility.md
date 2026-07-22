---
slug: privacy-security-and-accessibility
product_generation: mds-3
package_slug: million-dollar-script
package_type: core
package_version: "3.0.0"
channel: main
access: public
audience: [site-owners, administrators, developers]
published: true
tags: [privacy, security, accessibility, operations]
---

# Privacy, Security, And Accessibility

Million Dollar Script stores the information needed to reserve inventory, process payment, manage placements, and display approved advertising. Site owners remain responsible for configuring WordPress, payment providers, analytics, email delivery, legal pages, and retention practices for their jurisdiction and use case.

This guidance is operational information, not legal advice.

## Privacy Responsibilities

- Publish accurate privacy, terms, refund, advertising, and contact pages before accepting orders.
- Collect only the customer and placement fields needed for the service being offered.
- Keep customer email addresses, order keys, manage tokens, API keys, license keys, payment references, and unpublished placement metadata private.
- Review each extension's privacy and retention settings. Extensions that collect submissions may add their own WordPress personal-data export and erasure support.
- Document external processors such as WooCommerce gateways, SMTP providers, analytics services, ImageGrid, map tiles, or hosted extension services.
- Use the WordPress privacy tools and provider dashboards that apply to the data being processed. A database or CSV export may contain personal information and must be handled securely.

## Security Checklist

- Use HTTPS for WordPress administration, checkout, order management, extension-server connections, and hosted rendering.
- Give administrators and API clients only the capabilities or scopes they need.
- Revoke unused API keys, license activations, tester access keys, and staff accounts promptly.
- Keep WordPress, Million Dollar Script, maintained extensions, WooCommerce, PHP, the database, and the operating system updated.
- Use tested backups, malware monitoring, rate limiting, and an SMTP provider appropriate for the site.
- Never paste secrets into public support requests, screenshots, analytics, or email templates.
- Test checkout and renewal callbacks with provider sandbox credentials before accepting live payments.

## Accessible Operation

Million Dollar Script uses labels, keyboard-operable controls, live status messages, responsive lists, and accessible names throughout its standard interfaces. Site themes and custom templates can still reduce accessibility.

Before launch:

1. Complete an order using only a keyboard.
2. Check focus visibility, error announcements, field labels, and logical tab order.
3. Verify text and control contrast in both the selected light and dark appearances.
4. Test the grid, order form, tooltips, account pages, legal pages, and extension forms at narrow mobile widths and at browser zoom levels up to 200%.
5. Add meaningful alternative text to placement images. Avoid putting essential text only inside an image.
6. Check custom popup templates and custom fields with a screen reader before publishing them.

If a theme overrides Million Dollar Script styles, keep the override scoped and preserve focus outlines, labels, status regions, and touch target sizes.

## Incident Response

If a key, token, customer record, or private document may have been exposed, revoke the credential or access immediately, preserve relevant audit information, investigate the affected records, and follow applicable notification requirements. Report a reproducible Million Dollar Script security issue privately through the contact method on the official site rather than posting exploit details publicly.
