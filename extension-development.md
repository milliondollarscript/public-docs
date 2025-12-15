# Extension Development

Learn how to build your own MDS extensions using the official skeleton plugin and best practices.

## Getting Started

### Use the Skeleton Plugin

The **MDS Skeleton** plugin provides a clean starting point for new extensions:

1. Copy the `mds-skeleton` folder from the extensions repository
2. Rename the folder and main PHP file to match your extension name
3. Update namespaces and text domain throughout
4. Run `composer install` to set up autoloading
5. Activate and test in WordPress

### File Structure

A typical MDS extension follows this structure:

```
my-extension/
├── my-extension.php           # Main plugin file (bootstrap)
├── README.md                  # Documentation
├── readme.txt                 # WordPress.org-style readme
├── composer.json              # Composer dependencies & autoload
├── assets/
│   ├── css/
│   │   ├── admin.css          # Admin styles
│   │   └── frontend.css       # Frontend styles
│   └── js/
│       ├── admin.js           # Admin JavaScript
│       └── frontend.js        # Frontend JavaScript
├── src/
│   └── Plugin.php             # Main plugin class
├── includes/                  # Additional PHP classes (alternative to src/)
├── languages/
│   └── my-extension.pot       # Translation template
└── tests/
    └── ...                    # PHPUnit test suite
```

## Essential Components

### Main Plugin File

The main plugin file bootstraps your extension:

```php
<?php
/**
 * Plugin Name: My MDS Extension
 * Plugin URI: https://example.com/my-extension
 * Description: Description of what your extension does.
 * Version: 1.0.0
 * Author: Your Name
 * Author URI: https://example.com
 * Text Domain: my-mds-extension
 * Domain Path: /languages
 * Requires at least: 6.7
 * Requires PHP: 8.1
 * License: GPL-2.0-or-later
 */

// Prevent direct access
if (!defined('ABSPATH')) {
    exit;
}

// Define constants
define('MY_EXT_VERSION', '1.0.0');
define('MY_EXT_DIR', plugin_dir_path(__FILE__));
define('MY_EXT_URL', plugin_dir_url(__FILE__));
define('MY_EXT_BASENAME', plugin_basename(__FILE__));

// Composer autoloader
if (file_exists(MY_EXT_DIR . 'vendor/autoload.php')) {
    require_once MY_EXT_DIR . 'vendor/autoload.php';
}

// Initialize after plugins are loaded (ensures MDS is available)
add_action('plugins_loaded', function () {
    // Check if MDS core is active
    if (!class_exists('MillionDollarScript')) {
        add_action('admin_notices', function () {
            echo '<div class="notice notice-error"><p>';
            echo esc_html__('My MDS Extension requires Million Dollar Script to be installed and activated.', 'my-mds-extension');
            echo '</p></div>';
        });
        return;
    }

    // Initialize the extension
    My_Extension\Plugin::instance();
});
```

### Plugin Class

The main plugin class handles initialization and hooks:

```php
<?php
namespace My_Extension;

class Plugin {
    private static ?Plugin $instance = null;

    public static function instance(): Plugin {
        if (null === self::$instance) {
            self::$instance = new self();
        }
        return self::$instance;
    }

    private function __construct() {
        $this->init_hooks();
    }

    private function init_hooks(): void {
        // Load translations
        add_action('init', [$this, 'load_textdomain']);

        // Admin hooks
        add_action('admin_menu', [$this, 'register_admin_page']);
        add_action('admin_enqueue_scripts', [$this, 'enqueue_admin_assets']);

        // Frontend hooks
        add_action('wp_enqueue_scripts', [$this, 'enqueue_frontend_assets']);

        // MDS integration
        add_action('mds_main_menu_extensions_submenu', [$this, 'add_menu_link']);

        // Register shortcode
        add_shortcode('my_extension', [$this, 'shortcode_callback']);
    }

    public function load_textdomain(): void {
        load_plugin_textdomain(
            'my-mds-extension',
            false,
            dirname(MY_EXT_BASENAME) . '/languages'
        );
    }

    public function register_admin_page(): void {
        add_submenu_page(
            'milliondollarscript',          // Parent slug
            __('My Extension', 'my-mds-extension'),  // Page title
            __('My Extension', 'my-mds-extension'),  // Menu title
            'manage_options',               // Capability
            'my-extension',                 // Menu slug
            [$this, 'render_admin_page']    // Callback
        );
    }

    public function add_menu_link(): void {
        if (!current_user_can('manage_options')) {
            return;
        }
        echo '<li><a href="' . esc_url(admin_url('admin.php?page=my-extension')) . '">'
            . esc_html__('My Extension', 'my-mds-extension')
            . '</a></li>';
    }

    public function render_admin_page(): void {
        // Check permissions
        if (!current_user_can('manage_options')) {
            wp_die(__('You do not have permission to access this page.', 'my-mds-extension'));
        }
        ?>
        <div class="wrap">
            <h1><?php echo esc_html(get_admin_page_title()); ?></h1>
            <!-- Your admin content here -->
        </div>
        <?php
    }

    public function enqueue_admin_assets(string $hook): void {
        // Only load on our admin page
        if ('million-dollar-script_page_my-extension' !== $hook) {
            return;
        }

        wp_enqueue_style(
            'my-extension-admin',
            MY_EXT_URL . 'assets/css/admin.css',
            [],
            MY_EXT_VERSION
        );

        wp_enqueue_script(
            'my-extension-admin',
            MY_EXT_URL . 'assets/js/admin.js',
            ['jquery'],
            MY_EXT_VERSION,
            true
        );
    }

    public function enqueue_frontend_assets(): void {
        wp_enqueue_style(
            'my-extension-frontend',
            MY_EXT_URL . 'assets/css/frontend.css',
            [],
            MY_EXT_VERSION
        );
    }

    public function shortcode_callback(array $atts): string {
        $atts = shortcode_atts([
            'title'   => __('Default Title', 'my-mds-extension'),
            'option'  => 'value',
        ], $atts, 'my_extension');

        ob_start();
        ?>
        <div class="my-extension-wrapper">
            <h3><?php echo esc_html($atts['title']); ?></h3>
            <!-- Shortcode output here -->
        </div>
        <?php
        return ob_get_clean();
    }
}
```

## Adding to MDS Menu

The recommended way to add admin links is via the Extensions dropdown:

```php
add_action('mds_main_menu_extensions_submenu', function () {
    if (!current_user_can('manage_options')) {
        return;
    }
    echo '<li><a href="' . esc_url(admin_url('admin.php?page=my-extension')) . '">'
        . esc_html__('My Extension', 'my-mds-extension')
        . '</a></li>';
});
```

This keeps extensions organized and doesn't clutter the WordPress sidebar.

## Using Carbon Fields

MDS uses Carbon Fields for options. You can add your own options:

```php
use Carbon_Fields\Container;
use Carbon_Fields\Field;

add_action('carbon_fields_register_fields', function () {
    Container::make('theme_options', __('My Extension Settings', 'my-mds-extension'))
        ->set_page_parent('milliondollarscript')
        ->add_fields([
            Field::make('text', 'my_ext_api_key', __('API Key', 'my-mds-extension'))
                ->set_help_text(__('Enter your API key here.', 'my-mds-extension')),

            Field::make('checkbox', 'my_ext_enabled', __('Enable Feature', 'my-mds-extension'))
                ->set_option_value('yes'),

            Field::make('select', 'my_ext_mode', __('Mode', 'my-mds-extension'))
                ->add_options([
                    'basic'    => __('Basic', 'my-mds-extension'),
                    'advanced' => __('Advanced', 'my-mds-extension'),
                ]),
        ]);
});

// Retrieve option values
$api_key = carbon_get_theme_option('my_ext_api_key');
$enabled = carbon_get_theme_option('my_ext_enabled');
```

## Creating Shortcodes

```php
public function __construct() {
    add_shortcode('my_extension', [$this, 'shortcode_callback']);
}

public function shortcode_callback($atts): string {
    $atts = shortcode_atts([
        'title'       => __('Default Title', 'my-mds-extension'),
        'show_option' => 'true',
        'class'       => '',
    ], $atts, 'my_extension');

    // Sanitize attributes
    $show_option = filter_var($atts['show_option'], FILTER_VALIDATE_BOOLEAN);
    $class = sanitize_html_class($atts['class']);

    ob_start();
    ?>
    <div class="my-extension <?php echo esc_attr($class); ?>">
        <h3><?php echo esc_html($atts['title']); ?></h3>
        <?php if ($show_option) : ?>
            <p><?php esc_html_e('Option is enabled', 'my-mds-extension'); ?></p>
        <?php endif; ?>
    </div>
    <?php
    return ob_get_clean();
}
```

## AJAX Handlers

```php
// Register AJAX actions
add_action('wp_ajax_my_extension_action', [$this, 'handle_ajax']);
add_action('wp_ajax_nopriv_my_extension_action', [$this, 'handle_ajax']); // For non-logged-in users

public function handle_ajax(): void {
    // Verify nonce
    if (!check_ajax_referer('my_extension_nonce', 'nonce', false)) {
        wp_send_json_error(['message' => __('Security check failed.', 'my-mds-extension')]);
    }

    // Check permissions if needed
    if (!current_user_can('manage_options')) {
        wp_send_json_error(['message' => __('Permission denied.', 'my-mds-extension')]);
    }

    // Process the request
    $data = isset($_POST['data']) ? sanitize_text_field($_POST['data']) : '';

    // Return response
    wp_send_json_success([
        'message' => __('Success!', 'my-mds-extension'),
        'data'    => $data,
    ]);
}
```

## Testing

The skeleton includes PHPUnit setup with Brain Monkey for WordPress mocks:

```bash
# Install dependencies
composer install

# Run tests
composer test
# or
./vendor/bin/phpunit
```

Example test:

```php
<?php
namespace My_Extension\Tests;

use PHPUnit\Framework\TestCase;
use Brain\Monkey;
use Brain\Monkey\Functions;

class PluginTest extends TestCase {
    protected function setUp(): void {
        parent::setUp();
        Monkey\setUp();
    }

    protected function tearDown(): void {
        Monkey\tearDown();
        parent::tearDown();
    }

    public function test_shortcode_returns_html(): void {
        Functions\when('shortcode_atts')->returnArg(1);
        Functions\when('esc_html')->returnArg(1);
        Functions\when('__')->returnArg(1);

        $plugin = new \My_Extension\Plugin();
        $output = $plugin->shortcode_callback(['title' => 'Test']);

        $this->assertStringContainsString('Test', $output);
    }
}
```

## Activation and Deactivation

Handle plugin lifecycle events:

```php
// In main plugin file
register_activation_hook(__FILE__, [My_Extension\Plugin::class, 'activate']);
register_deactivation_hook(__FILE__, [My_Extension\Plugin::class, 'deactivate']);

// In Plugin class
public static function activate(): void {
    // Create database tables
    // Set default options
    // Flush rewrite rules if registering custom post types
    flush_rewrite_rules();
}

public static function deactivate(): void {
    // Clean up scheduled events
    wp_clear_scheduled_hook('my_extension_cron');
    // Flush rewrite rules
    flush_rewrite_rules();
}
```

## Uninstall Cleanup

Create `uninstall.php` in your plugin root:

```php
<?php
// Exit if not called by WordPress
if (!defined('WP_UNINSTALL_PLUGIN')) {
    exit;
}

// Delete options
delete_option('my_extension_settings');

// Delete custom tables (if any)
global $wpdb;
$wpdb->query("DROP TABLE IF EXISTS {$wpdb->prefix}my_extension_data");

// Delete user meta (if any)
delete_metadata('user', 0, 'my_extension_preference', '', true);
```

## Release Checklist

Before releasing your extension:

- [ ] Unique text domain that won't conflict with other plugins
- [ ] All strings wrapped in translation functions
- [ ] Proper capability checks on all admin functions
- [ ] Nonce verification on all form submissions
- [ ] Input sanitization on all user data
- [ ] Output escaping on all rendered content
- [ ] Activation/deactivation hooks handle setup/cleanup
- [ ] Uninstall.php removes all plugin data
- [ ] README.md with installation and usage instructions
- [ ] Changelog documenting all versions
- [ ] Tests passing
- [ ] Code follows WordPress Coding Standards
- [ ] Tested with latest WordPress and PHP versions
- [ ] Tested with latest MDS version

## Resources

- [Hooks Reference](/docs/hooks-reference) - Available MDS hooks
- [List Page Customization](/docs/list-page-customization) - Extending the advertiser list
- [WordPress Plugin Handbook](https://developer.wordpress.org/plugins/)
- [Carbon Fields Documentation](https://carbonfields.net/docs/)
