# Developer Overview

This guide introduces developers to extending Million Dollar Script. Whether you're building custom features for your site or creating shareable extensions, MDS provides hooks, filters, and patterns to integrate seamlessly.

## Architecture Overview

MDS follows WordPress conventions and best practices:

- **Hooks and filters** for extensibility at key points
- **Proper capability checks** before rendering admin content
- **Nonce verification** for all form submissions
- **Input sanitization** and output escaping
- **Translation-ready strings** using WordPress i18n functions

The plugin is built around a modular class structure with namespaced PHP (PSR-4) and Composer autoloading.

## Key Concepts

### Text Domains

- **Core plugin:** `milliondollarscript-two`
- **Extensions:** Each extension uses its own text domain (e.g., `mds-translation`, `mds-fields`)

Always use the appropriate text domain for translatable strings.

### Database

MDS uses WordPress database conventions:

- Database operations use `$wpdb` directly
- Custom tables use `{$wpdb->prefix}mds_*` naming
- Plugin options are stored in the WordPress options table
- The `mds-pixel` custom post type stores pixel/advertiser data

### Admin Integration

Extensions integrate with MDS admin in several ways:

- `mds_main_menu_extensions_submenu` - Add items to the Extensions dropdown (recommended)
- `mds_main_menu_admin_submenu` - Add items to the Admin dropdown
- `mds_main_menu_top` - Add top-level hero menu items

See [Hooks Reference](/docs/hooks-reference) for complete documentation.

## Getting Started

### Recommended Reading

1. **[Hooks Reference](/docs/hooks-reference)** - Available actions and filters
2. **[Extension Development](/docs/extension-development)** - Building your own extension
3. **[List Page Customization](/docs/list-page-customization)** - Extending the advertiser list

### Example Extensions

Two example extensions demonstrate MDS development patterns:

- **MDS Sample Greeter** - Basic extension with admin page, shortcode, and AJAX
- **MDS Skeleton** - Clean boilerplate for scaffolding new extensions

Both are available in the MDS extensions repository and can serve as starting points.

### Development Environment

Recommended setup:

- Local WordPress installation (Local by Flywheel, DDEV, or Docker)
- PHP 8.1+ with debugging enabled
- WordPress debug constants in `wp-config.php`:
  ```php
  define('WP_DEBUG', true);
  define('WP_DEBUG_LOG', true);
  define('WP_DEBUG_DISPLAY', false);
  ```
- Million Dollar Script core plugin installed and activated
- Code editor with PHP support (VS Code, PhpStorm)

### Quick Test

To verify your environment is ready for MDS development:

1. Activate MDS and confirm the admin menu appears
2. Check `wp-content/debug.log` exists and is writable
3. Test a simple hook:
   ```php
   add_action('mds_main_menu_extensions_submenu', function () {
       if (current_user_can('manage_options')) {
           echo '<li><a href="#">Dev Test</a></li>';
       }
   });
   ```
4. Verify "Dev Test" appears in the Extensions menu

## Coding Standards

MDS follows WordPress Coding Standards:

- **PHP:** [WordPress PHP Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/)
- **JavaScript:** [WordPress JavaScript Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/javascript/)
- **CSS:** [WordPress CSS Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/css/)

Key practices:

- Use meaningful, descriptive names for functions, classes, and variables
- Prefix all function names, classes, and global variables with your extension's unique prefix
- Always escape output (`esc_html()`, `esc_attr()`, `esc_url()`)
- Always sanitize input (`sanitize_text_field()`, `absint()`, etc.)
- Use nonces for form verification (`wp_nonce_field()`, `wp_verify_nonce()`)
- Check capabilities before performing privileged actions (`current_user_can()`)

## Requirements

Extensions should declare their requirements in the plugin header:

```php
/**
 * Plugin Name: My MDS Extension
 * Requires at least: 6.7
 * Requires PHP: 8.1
 */
```

Minimum requirements for MDS compatibility:

- WordPress 6.7+
- PHP 8.1+
- Million Dollar Script core plugin (active)

## Support Resources

- **GitHub Issues** - Report bugs or request features
- **Discord Community** - Developer discussions
- **This Documentation** - Reference guides and examples
