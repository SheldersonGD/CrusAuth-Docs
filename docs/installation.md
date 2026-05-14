# Installation Guide

This guide covers the two supported ways to run CrusAuth:

1. **Standalone Paper/Folia setup** — cracked players register and log in on one backend server.
2. **Hybrid proxy setup** — Velocity or BungeeCord decides whether a connection is premium or cracked, while Paper/Folia handles the backend auth session.

For hybrid premium/cracked networks, use the proxy setup. A standalone offline-mode Paper/Folia server cannot securely prove Mojang account ownership by itself.

## Requirements

| Requirement | Notes |
|---|---|
| Java 21+ | Required by the plugin build. |
| Paper or Folia backend | The core plugin runs on the backend server. |
| Valid CrusAuth license key | Required before the Paper/Folia core enables. |
| Velocity addon | Required only for Velocity hybrid networks. |
| BungeeCord addon | Required only for BungeeCord hybrid networks. |
| MySQL/MariaDB | Optional. SQLite is used by default. |

## Files you receive

A typical release package contains:

```text
CrusAuth-Paper-x.x.x.jar       # Main backend plugin
CrusAuth-Velocity-x.x.x.jar    # Optional Velocity addon
CrusAuth-Bungee-x.x.x.jar      # Optional BungeeCord addon
```

Install only the components that match your network.

## Standalone Paper/Folia setup

Use this when you only need cracked `/register` and `/login` on one backend server.

```text
Player
  ↓
Paper/Folia server
  ↓
SQLite accounts.db
```

Steps:

1. Stop the server.
2. Put `CrusAuth-Paper-x.x.x.jar` into the backend `plugins/` folder.
3. Start the server once.
4. Open `plugins/CrusAuth/config.yml`.
5. Set your license key:

```yaml
license:
  key: "YOUR_LICENSE_KEY"
```

6. Keep SQLite enabled unless you need MySQL/MariaDB:

```yaml
storage:
  type: sqlite
```

7. Restart the server.
8. Join with a cracked/offline account and test `/register` and `/login`.

### Standalone premium limitation

If the backend is offline-mode and there is no proxy performing Mojang authentication, CrusAuth cannot securely verify that a player owns a premium account. In that setup, premium players should log in normally like cracked players.

If the Paper server itself runs in `online-mode=true`, CrusAuth can trust joining players as premium when this setting is enabled:

```yaml
standalone:
  auto-login-online-mode-players: true
```

Do not enable username-only premium auto-login on public offline-mode servers unless you fully understand the spoofing risk.

## Recommended hybrid setup

Use this for premium auto-login, cracked login/register support, premium name protection, and proxy-side decision handling.

```text
Player
  ↓
Velocity or BungeeCord
  ↓
Paper/Folia auth backend
  ↓
SQLite or MySQL/MariaDB
```

Steps:

1. Install `CrusAuth-Paper-x.x.x.jar` on the backend auth/lobby server.
2. Install either `CrusAuth-Velocity-x.x.x.jar` or `CrusAuth-Bungee-x.x.x.jar` on the proxy.
3. Start proxy and backend once to generate configs.
4. Set the license key in the Paper/Folia backend config.
5. Generate one long random bridge secret.
6. Put the same bridge secret in both the backend and proxy config.
7. Configure the proxy module's trusted backend server list.
8. Restart proxy and backend.
9. Test premium and cracked login flows.

## Bridge secret

The bridge secret protects communication between the proxy addon and the Paper/Folia core.

Generate a long random value. Examples:

```bash
openssl rand -base64 48
```

PowerShell:

```powershell
[Convert]::ToBase64String((1..48 | ForEach-Object { Get-Random -Minimum 0 -Maximum 256 }))
```

Set it on Paper/Folia:

```yaml
security:
  bridge-secret: "PASTE_THE_SAME_LONG_RANDOM_SECRET_HERE"
```

Set it on Velocity or BungeeCord:

```yaml
bridge-secret: "PASTE_THE_SAME_LONG_RANDOM_SECRET_HERE"
```

Never post your bridge secret publicly.

## Trusted backend servers

The proxy module only accepts signed auth-state bridge messages from trusted backend servers.

Example:

```yaml
trusted-backend-servers:
  - auth
```

The value must match the server name in your Velocity or BungeeCord proxy configuration.

## Sending players to the lobby after login

If CrusAuth runs on a dedicated auth server and you want players transferred to a lobby after successful login/register/premium auto-login:

```yaml
proxy-forwarding:
  connect-after-auth: true
  target-server: "lobby"
  connect-delay-ticks: 10
```

Make sure `target-server` matches the proxy server name exactly.

## First test checklist

Before opening the server publicly, test these cases:

- Fresh cracked username: should require `/register`.
- Registered cracked username: should require `/login`.
- Wrong password attempts: should kick or temporarily block after the configured limit.
- Premium username with a real premium client: should auto-login when using the proxy addon.
- Premium-claimed username with a cracked launcher: should be blocked before backend gameplay.
- Cracked-first premium-looking username: should still require `/login` and should not be silently taken over by a premium client.
- Backend direct join: should be impossible from the internet.
- Proxy server switching before authentication: should be denied.
