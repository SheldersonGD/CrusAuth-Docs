# CrusAuth Documentation

CrusAuth is a hybrid authentication system for modern Minecraft servers and networks. It provides cracked player registration/login on Paper and Folia, with optional Velocity and BungeeCord proxy modules for premium auto-login, premium name protection, and proxy-aware account handling.

This repository contains the public documentation for CrusAuth. It does not contain the plugin source code, license backend, private keys, obfuscation mappings, build scripts, or customer data.

## Quick links

- [Installation Guide](docs/installation.md)
- [Paper/Folia Setup](docs/paper-folia-setup.md)
- [Velocity Setup](docs/velocity-setup.md)
- [BungeeCord Setup](docs/bungeecord-setup.md)
- [Storage: SQLite, MySQL and MariaDB](docs/storage.md)
- [Commands and Permissions](docs/commands-permissions.md)
- [Premium & Cracked Account Behavior](docs/premium-cracked-behavior.md)
- [Security Notes](docs/security.md)
- [License Information](docs/license.md)
- [Troubleshooting](docs/troubleshooting.md)
- [Support Guide](docs/support.md)

## Platform support

| Component | Purpose | Included in base resource |
|---|---:|---:|
| Paper/Folia Core | Backend authentication, player restrictions, storage, commands | Yes |
| Velocity Module | Hybrid premium/cracked proxy decision layer | Optional addon |
| BungeeCord Module | Hybrid premium/cracked proxy decision layer | Optional addon |
| SQLite | Local account storage | Yes |
| MySQL/MariaDB | Shared network storage | Yes |

## Purchase and support

- [BuiltByBit](https://builtbybit.com/resources/crusauth-hybrid-authentication.107784/)
- [Discord](https://discord.gg/cdjjkRpNqr)

Keep license keys, bridge secrets, database passwords, account databases, and log files private when requesting support.
