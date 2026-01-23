# wp network

Perform network-wide operations.

## Description

The `wp network` command provides tools to manage and configure WordPress multisite network settings. This command group handles network-level meta fields, options, and configurations that apply across all sites in your multisite installation.

Use this command when you need to work with data at the network level rather than individual site levels. Common operations include managing network metadata, site administrators, and other network-wide settings.

## Subcommands

- **[wp network meta](#wp-network-meta)** - Gets, adds, updates, deletes, and lists network custom fields
- **[wp network option](#wp-network-option)** - Gets and sets network-wide options

## Global Options

| Option | Description |
|--------|-------------|
| `--debug` | Show all PHP errors and add verbosity to WP-CLI output |
| `--quiet` | Suppress all output |
| `--allow-root` | Allow WordPress to run as the root user |

## Examples

### Get network-wide metadata

```bash
$ wp network meta get 1 site_admins
array (
  0 => 'supervisor',
)
```

### List all network metadata

```bash
$ wp network meta list 1
+------------------+-------------------+
| meta_key         | meta_value        |
+------------------+-------------------+
| site_admins      | a:1:{i:0;s:10:"supervisor";} |
| admin_email      | admin@example.com |
+------------------+-------------------+
```

## Notes

- Network commands only work on WordPress multisite installations
- Network ID is typically `1` for the primary network
- The `wp network meta` command is commonly used for managing super-admin lists and network-wide metadata

## Resources

- [WordPress Multisite Documentation](https://developer.wordpress.org/advanced-administration/multisite/)