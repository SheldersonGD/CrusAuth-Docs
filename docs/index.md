# CrusAuth Documentation

CrusAuth is a modern authentication system for Minecraft servers that need cracked account login, premium auto-login, and proxy-aware account protection.

The main resource includes the Paper/Folia backend plugin. Velocity and BungeeCord support are provided as optional paid addons.

## Documentation sections

| Section | Description |
|---|---|
| [Installation Guide](installation.md) | Recommended setup paths for standalone and proxy networks. |
| [Paper/Folia Setup](paper-folia-setup.md) | Backend installation, limbo, spawn behavior, sessions, restrictions. |
| [Velocity Setup](velocity-setup.md) | Velocity proxy module installation and configuration. |
| [BungeeCord Setup](bungeecord-setup.md) | BungeeCord proxy module installation and compatibility notes. |
| [Storage](storage.md) | SQLite default storage and optional MySQL/MariaDB setup. |
| [Commands and Permissions](commands-permissions.md) | Player, admin and proxy commands. |
| [Premium & Cracked Behavior](premium-cracked-behavior.md) | How account ownership and premium name protection work. |
| [Security Notes](security.md) | Bridge secrets, account safety, backend firewalling, safe disclosures. |
| [License Information](license.md) | License key setup and proxy addon licensing. |
| [Troubleshooting](troubleshooting.md) | Common installation and runtime issues. |
| [Support Guide](support.md) | What to include when asking for help. |

## Recommended network layout

```text
Player
  ↓
Velocity / BungeeCord
  ↓
Paper / Folia auth backend
  ↓
SQLite or MySQL/MariaDB
```

For a single offline-mode server, the Paper/Folia core can also be used as a standalone cracked authentication plugin.
