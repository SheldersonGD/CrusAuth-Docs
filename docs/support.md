# Support Guide

This page explains what to provide when requesting support.

## Before asking for help

Please check:

- [Installation Guide](installation.md)
- [Velocity Setup](velocity-setup.md)
- [BungeeCord Setup](bungeecord-setup.md)
- [Storage](storage.md)
- [Troubleshooting](troubleshooting.md)

Many problems are caused by mismatched bridge secrets, wrong proxy backend names, missing license keys, direct backend access, or MySQL configuration mistakes.

## Include this information

When opening a support request, include:

| Field | Example |
|---|---|
| CrusAuth version | `1.0.0` |
| Backend platform | `Paper 1.21.x` or `Folia 1.21.x` |
| Proxy platform | `Velocity` / `BungeeCord` / none |
| Java version | `Java 21` |
| Storage type | SQLite / MySQL / MariaDB |
| Setup type | standalone / proxy auth server / proxy lobby |
| Problem | what happened and what you expected |
| Reproduction steps | exact steps to trigger the issue |
| Logs | startup and error logs with secrets removed |

## Redact secrets

Before posting configs or logs, remove:

- license keys;
- bridge secrets;
- database passwords;
- IP addresses if private;
- server IDs;
- account database files;
- license cache files;
- offline license slot files.

Do not upload your whole server folder.

## Useful files

Safe to share after redaction:

```text
plugins/CrusAuth/config.yml
plugins/CrusAuth/messages.yml
latest.log
proxy startup log
```

Do not share:

```text
plugins/CrusAuth/accounts.db
plugins/CrusAuth/accounts.yml
plugins/CrusAuth/license-cache.properties
plugins/CrusAuth/offline-license-slots.properties
plugins/CrusAuth/server-id.txt
```

## Good bug report format

```text
CrusAuth version:
Backend platform/version:
Proxy platform/version:
Java version:
Storage type:
Setup type:

What I expected:
What happened:
Steps to reproduce:
Relevant config sections:
Relevant logs:
```

## Support links

- BuiltByBit resource: `PUT_BUILTBYBIT_RESOURCE_LINK_HERE`
- Discord support: `PUT_SUPPORT_DISCORD_LINK_HERE`
