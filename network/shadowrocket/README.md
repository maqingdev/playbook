# Shadowrocket Config

This directory stores my shared Shadowrocket configuration for macOS and iOS.

## Files

### `maqing.conf`

Main Shadowrocket configuration file.

Raw URL:

```text
https://raw.githubusercontent.com/maqingdev/playbook/main/network/shadowrocket/maqing.conf
```

GitHub URL:

```text
https://github.com/maqingdev/playbook/blob/main/network/shadowrocket/maqing.conf
```

## How to use

In Shadowrocket, import the config from URL:

```text
https://raw.githubusercontent.com/maqingdev/playbook/main/network/shadowrocket/maqing.conf
```

After updating this file in GitHub, manually update or re-import the config in Shadowrocket.

## Current rule strategy

The config is designed to:

- Direct company domains:
  - `lundbecksys.cn`
  - `lekinsights.cn`
  - `lekdigital.cn`
- Direct MySQL traffic on ports `3306` and `3307`
- Direct Apple / China iCloud / software update traffic
- Direct Microsoft Teams traffic, including chat, calls, and meeting media
- Direct China mainland IP traffic with `GEOIP,CN,DIRECT`
- Proxy all remaining traffic with `FINAL,PROXY`

## External rule sets

The config may reference maintained Shadowrocket rule sets from:

```text
https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Shadowrocket
```

Examples:

```text
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Shadowrocket/Apple/Apple.list
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Shadowrocket/Teams/Teams.list
```

## Security notes

This repository is public.

Do not commit:

- Real proxy nodes
- UUIDs
- Passwords
- Tokens
- Private subscription URLs
- Any credentials or secrets
