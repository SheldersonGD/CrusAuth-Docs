# Troubleshooting

This page lists common CrusAuth setup issues and the most likely fixes.

## Paper/Folia plugin disables on startup

Common causes:

- missing license key;
- invalid license key;
- no outbound connection to the license validation service;
- corrupted local license cache.

Check:

```yaml
license:
  key: "YOUR_LICENSE_KEY"
```

Restart after changing the key.

## Premium players see `/login` or `/register`

Most likely causes:

- proxy addon is not installed;
- proxy addon is not licensed;
- bridge secret does not match between proxy and backend;
- `trusted-backend-servers` does not contain the correct auth backend name;
- player connected directly to the backend instead of through the proxy;
- Mojang lookup failed and fallback-to-offline is enabled;
- auth prompt delay was reduced too aggressively.

Check backend config:

```yaml
security:
  bridge-secret: "..."
```

Check proxy config:

```yaml
bridge-secret: "..."
trusted-backend-servers:
  - auth
```

The backend and proxy values must match exactly.

## Cracked player with a premium-claimed username gets invalid session

This is expected when premium name protection is enabled.

Once the real premium owner has claimed the username, cracked clients using that visible name are forced through official premium authentication and should fail before backend gameplay.

Relevant setting:

```yaml
premium-name-policy:
  protect-claimed-premium-names: true
```

## Real premium owner must `/login`

This can be expected if the cracked/password account registered the name first.

Relevant setting:

```yaml
premium-name-policy:
  cracked-registered-account-wins: true
```

Backend setting:

```yaml
security:
  cracked-registered-account-wins-over-auto-premium: true
```

This prevents premium accounts from silently taking over older cracked/password accounts.

## Proxy says addon is not enabled on this license

The installed proxy module is not included in the active license.

Fix:

1. Confirm the backend has the correct license key.
2. Confirm the purchased addon matches your proxy platform.
3. Restart the Paper/Folia backend.
4. Restart the proxy.
5. Join once through the proxy so the backend can send signed license status.

If the issue remains, contact support with startup logs from both proxy and backend.

## Players can bypass authentication by connecting to backend

This is a network configuration issue.

Fix:

- firewall backend ports;
- expose only the proxy port;
- restrict backend access to the proxy IP;
- do not publish backend IPs;
- verify backend direct joins fail from an external machine.

CrusAuth cannot secure a backend that players can directly join around the proxy.

## Folia cannot create limbo world

Folia does not support runtime world creation in the same way Paper does.

Fix options:

- pre-create the limbo world and set `limbo.folia-enabled: true`;
- use a fixed spawn instead of limbo;
- use `spawn.pre-auth-mode: "spawn"` or `"last-location"`.

## Player falls or gets void issues before login

Check limbo settings:

```yaml
limbo:
  create-platform: true
  platform-material: "BARRIER"
  disable-gravity: true
  invulnerable: true
  keep-in-void: true
```

On Folia, make sure the limbo world already exists and is loaded before enabling Folia limbo.

## MySQL is slow or unreachable on startup

CrusAuth uses SQLite by default. MySQL is contacted only when enabled.

If MySQL is configured but unreachable, CrusAuth can fall back to SQLite:

```yaml
storage:
  fallback-to-sqlite-on-sql-failure: true
```

Check:

- host and port;
- database name;
- username and password;
- firewall rules;
- database user privileges;
- connection limit;
- whether the password is still `CHANGE_ME`.

## Players are temporarily blocked after wrong passwords

This is expected after too many failed login attempts.

Relevant settings:

```yaml
security:
  max-login-attempts: 4
  failed-login-block-seconds: 30
```

Admin reset:

```text
/crusauth resetattempts <player>
```

## Auth commands appear in console logs

Make sure sensitive command suppression is enabled:

```yaml
security:
  suppress-sensitive-command-logs: true
```

Restart after changing it.

## Bungee premium protection does not behave like Velocity

BungeeCord has weaker public API support for per-connection online/offline switching. CrusAuth uses a common internal Bungee method, but some forks may block or remove it.

If your Bungee fork is incompatible:

- use Velocity for the hybrid proxy layer, or
- use a compatible Bungee fork, or
- disable premium name protection only if you accept the security tradeoff.

Velocity is recommended for the cleanest hybrid premium/cracked behavior.
