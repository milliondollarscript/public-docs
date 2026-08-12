---
slug: customization-localization-and-templates
navigation_title: Customization and localization
product_generation: mds-2
package_slug: million-dollar-script
package_type: core
package_version: "2.6"
channel: main
access: public
audience: [site-owners, administrators, developers]
published: true
tags: [mds2, customization, translation, themes, templates]
---

# Customization, Localization, and Templates

Customize the current MDS 2 WordPress plugin with WordPress pages, blocks, shortcodes, themes, translations, and documented hooks. Avoid editing plugin files because an update will replace those changes.

## Headers, footers, and page layout

The WordPress theme controls the site's header and footer. The block editor or theme templates control the content around a Million Dollar Script block or shortcode.

The archived standalone script used files such as `html/header.php` and `html/footer.php`. Those files are not the header and footer for the WordPress plugin. Follow the documentation for the active theme, using a child theme when code changes are required.

## Translate the plugin

Set the site language under **Settings > General** and install available language packs through WordPress. For an untranslated string, create or contribute a standard WordPress translation for the Million Dollar Script text domain.

Store custom translations in the location recommended by the translation tool so plugin updates do not overwrite them. Do not edit the distributed language file directly.

The standalone script's PHP language arrays apply only to the matching archived release. They cannot be copied into the WordPress plugin or MDS 3.0.

## Advanced customization

Use this order of preference:

1. Plugin settings and grid settings.
2. WordPress blocks and shortcodes.
3. Theme or child-theme CSS.
4. Documented actions and filters in a small site plugin.
5. A dedicated Million Dollar Script extension for reusable behavior.

Take a backup and test customizations on a staging site before updating production. If a change requires replacing a core class, editing database queries, or altering payment state directly, treat it as custom development that needs security and upgrade review.

## Version boundaries

MDS 2 WordPress hooks and templates are not MDS 3.0 APIs. MDS 3.0 extensions must use its version-neutral public facades and current extension documentation. The archived standalone MDS 2 templates are a third, separate system and should only be maintained with files from their exact release.
