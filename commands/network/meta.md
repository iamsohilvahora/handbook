# wp network meta

Gets, adds, updates, deletes, and lists network custom fields.

## Description

The `wp network meta` command manages custom metadata (custom fields) at the network level in WordPress multisite installations. Network metadata is stored in the `wp_networkmeta` table and is accessible across all sites in the network.

This is useful for storing network-wide configuration, site administrator lists, feature flags, and other custom data that should persist at the network level rather than individual site levels.

## Subcommands

### wp network meta get

Gets the value of a specific network meta field.

#### Usage

```bash
wp network meta get <network-id> <key> [--format=<format>]
```

#### Arguments

| Argument | Description | Required |
|----------|-------------|----------|
| `<network-id>` | The ID of the network to get metadata from (typically `1`) | Yes |
| `<key>` | The meta key to retrieve | Yes |

#### Options

| Option | Description | Type | Default |
|--------|-------------|------|---------|
| `--format` | Output format (`var_export`, `json`, `yaml`) | string | var_export |

### wp network meta set

Sets the value of a network meta field, creating it if it doesn't exist.

#### Usage

```bash
wp network meta set <network-id> <key> <value> [--format=<format>]
```

#### Arguments

| Argument | Description | Required |
|----------|-------------|----------|
| `<network-id>` | The ID of the network | Yes |
| `<key>` | The meta key to set | Yes |
| `<value>` | The meta value to set | Yes |

### wp network meta update

Updates an existing network meta field.

#### Usage

```bash
wp network meta update <network-id> <key> <value> [--format=<format>]
```

### wp network meta delete

Deletes a network meta field.

#### Usage

```bash
wp network meta delete <network-id> <key>
```

#### Arguments

| Argument | Description | Required |
|----------|-------------|----------|
| `<network-id>` | The ID of the network | Yes |
| `<key>` | The meta key to delete | Yes |

### wp network meta list

Lists all metadata for a network.

#### Usage

```bash
wp network meta list <network-id> [--keys=<keys>] [--fields=<fields>] [--format=<format>]
```

#### Arguments

| Argument | Description | Required |
|----------|-------------|----------|
| `<network-id>` | The ID of the network (typically `1`) | Yes |

#### Options

| Option | Description | Type |
|--------|-------------|------|
| `--keys` | Limit to specific meta keys (comma-separated) | string |
| `--fields` | Which fields to display (`meta_id`, `meta_key`, `meta_value`) | string |
| `--format` | Output format (`table`, `csv`, `json`, `count`) | string |

## Examples

### Get a list of super-admins

```bash
$ wp network meta get 1 site_admins
array (
  0 => 'supervisor',
)
```

### Get network metadata in JSON format

```bash
$ wp network meta get 1 site_admins --format=json
["supervisor","admin2"]
```

### Set a network-wide setting

```bash
$ wp network meta set 1 network_setting "value123"
Success: Updated network meta.
```

### Update a network meta field

```bash
$ wp network meta update 1 site_admins 'a:2:{i:0;s:10:"supervisor";i:1;s:5:"admin2";}'
Success: Updated network meta.
```

### List all network metadata

```bash
$ wp network meta list 1
+------------------+-----------------------------------------------+
| meta_key         | meta_value                                    |
+------------------+-----------------------------------------------+
| site_admins      | a:1:{i:0;s:10:"supervisor";}                 |
| siteurl          | https://example.com                           |
| admin_email      | admin@example.com                             |
+------------------+-----------------------------------------------+
```

### List specific network metadata fields as CSV

```bash
$ wp network meta list 1 --keys=site_admins,admin_email --format=csv
meta_key,meta_value
site_admins,"a:1:{i:0;s:10:""supervisor"";}"
admin_email,"admin@example.com"
```

### Get count of all network meta fields

```bash
$ wp network meta list 1 --format=count
3
```

### Delete a network meta field

```bash
$ wp network meta delete 1 old_setting
Success: Deleted network meta.
```

### Add a new super-admin to the network

```bash
$ wp network meta get 1 site_admins
array (
  0 => 'supervisor',
)
$ wp network meta set 1 site_admins 'a:2:{i:0;s:10:"supervisor";i:1;s:9:"newadmin";}'
Success: Updated network meta.
```

## Common Use Cases

### Manage network super-admins programmatically

```bash
# Get current super-admins
wp network meta get 1 site_admins

# Update super-admin list (serialized array)
wp network meta set 1 site_admins 'a:2:{i:0;s:5:"admin";i:1;s:6:"admin2";}'
```

### Export network metadata for backup

```bash
wp network meta list 1 --format=json > network_meta_backup.json
```

### Verify network-wide settings

```bash
wp network meta list 1 --fields=meta_key,meta_value
```

### Migrate network settings between installations

```bash
# Export from source
wp network meta list 1 --format=json > settings.json

# Import to target (requires custom script)
# Parse JSON and use wp network meta set commands
```

## Tips & Best Practices

1. **Network ID**: Network ID is typically `1` for standard WordPress multisite installations
2. **Serialized Data**: Some meta values like `site_admins` are PHP-serialized arrays. Use proper serialization when setting values
3. **Backup First**: Always backup the `wp_networkmeta` table before bulk operations
4. **Use Consistent Format**: When working with programmatic data, use `--format=json` for easier parsing
5. **Admin Changes**: When modifying `site_admins`, ensure you don't lock yourself out of network administration
6. **Test in Development**: Test network meta changes on a development/staging network first

## Field Reference

### Common Network Meta Keys

| Meta Key | Description | Format |
|----------|-------------|--------|
| `site_admins` | Network super-administrators | PHP serialized array |
| `siteurl` | Network/primary site URL | String |
| `home` | Network home URL | String |
| `admin_email` | Network administrator email | String |
| `blogs_limit` | Maximum blogs per site | Integer |

## Related Commands

- `wp network` - Perform network-wide operations
- `wp site meta` - Manage site-specific custom fields
- `wp user meta` - Manage user metadata
- `wp option` - Manage site options
- `wp network option` - Manage network options

## Notes

- Network metadata is stored in the `wp_networkmeta` database table
- Only network administrators can view and modify network-level metadata
- Metadata values larger than 4MB may not be stored correctly
- Serialized arrays are common in network metadata; handle with care when updating programmatically
- Changes to sensitive meta fields like `site_admins` should be logged and documented