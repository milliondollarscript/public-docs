---
slug: email-delivery
navigation_title: Email delivery
product_generation: mds-2
package_slug: million-dollar-script
package_type: core
package_version: "2.6"
channel: main
access: public
audience: [site-owners, administrators]
published: true
tags: [mds2, email, smtp, troubleshooting]
---

# Email Delivery and SMTP Troubleshooting

The current MDS 2 WordPress plugin sends mail through the WordPress mail system. Configure delivery for the whole WordPress site instead of editing Million Dollar Script files or PHP's global mail configuration.

## Recommended setup

1. Choose a reputable transactional email provider or the SMTP service supplied by your host.
2. Configure it with a maintained WordPress SMTP or mail-delivery plugin.
3. Use the provider's required encryption, port, and authentication method.
4. Send a test message from the mail plugin.
5. Complete a Million Dollar Script test order and confirm the expected customer and administrator messages.
6. Configure SPF, DKIM, and DMARC for the sending domain when the provider supports them.

Keep credentials in the mail plugin or the host's secret configuration. Do not paste SMTP passwords into a theme, custom snippet, public support request, or Million Dollar Script template.

## Connection refused

`Connection refused` means the site could not open a connection to the configured mail server and port. Check:

- the SMTP hostname and port;
- whether the port expects TLS, STARTTLS, or an unencrypted local connection;
- outbound-port restrictions imposed by the host;
- a local firewall or container network rule;
- whether the mail service is actually listening;
- provider account restrictions or an incorrect region-specific hostname.

A timeout usually indicates a network or firewall path problem. An authentication error indicates that the server was reached but rejected the credentials or method.

## Local Windows development

Do not configure a developer workstation as an open mail relay. For local WordPress development, use a mail-capture service such as Mailpit or MailHog, or use the sandbox mode provided by a transactional mail service. Captured messages stay local and cannot accidentally reach customers.

## Older standalone instructions

Historical articles changed `php.ini` or the standalone script's mail settings. Those steps do not configure the current WordPress plugin. A legacy standalone installation should be isolated and maintained with the PHP and mail documentation for that exact environment.

MDS 3.0 also sends through WordPress unless an installed extension explicitly documents another delivery provider. Use its package-specific documentation for extension-owned settings.
