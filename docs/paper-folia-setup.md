# Paper/Folia Setup

The Paper/Folia plugin is the required backend component of CrusAuth. It handles account storage, password hashing, authentication sessions, player restrictions, limbo/spawn behavior, admin commands, and signed communication with proxy addons.

## Installation

1. Stop the backend server.
2. Place `CrusAuth-Paper-x.x.x.jar` in `plugins/`.
3. Start the server once to generate `plugins/CrusAuth/config.yml` and `messages.yml`.
4. Add your license key.
5. Restart the server.

```yaml
license:
  key: "YOUR_LICENSE_KEY"
```

If the license is missing or invalid, the Paper/Folia plugin disables itself.

## Backend network mode

For proxy networks, the backend should not be publicly reachable. Players must connect through Velocity or BungeeCord.

Recommended production rules:

- backend `server.properties` uses offline-mode when behind a proxy;
- proxy forwarding is configured according to your proxy platform;
- backend ports are blocked from public access;
- only the proxy can connect to backend servers;
- the same bridge secret is configured on proxy and backend.

Direct backend access bypasses the proxy decision layer and should be treated as a security issue.

## Security section

Important backend security settings:

```yaml
security:
  bridge-secret: "CHANGE_ME_TO_A_LONG_RANDOM_SECRET_32_CHARS_MIN"
  bridge-message-max-age-ms: 10000
  min-password-length: 6
  max-password-length: 64
  max-login-attempts: 4
  failed-login-block-seconds: 30
  login-timeout-seconds: 60
  password-hash-iterations: 210000
  max-accounts-per-ip: 3
  suppress-sensitive-command-logs: true
  block-invalid-usernames: true
```

Recommended production values:

- Change `bridge-secret` before using a proxy addon.
- Keep `suppress-sensitive-command-logs: true`.
- Keep `block-invalid-usernames: true`.
- Keep a reasonable password length policy.
- Keep `max-login-attempts` low enough to limit brute-force attempts.

## Auth worker settings

Password hashing runs off the main thread through a bounded worker queue:

```yaml
auth-worker-threads: 2
auth-queue-limit: 128
auth-rate-limit:
  window-seconds: 10
  per-ip-attempts: 8
  global-attempts: 80
```

For most servers, the defaults are safe. Increase worker threads only if your auth backend has enough CPU headroom and you have measured real login pressure.

## Sessions

Sessions allow recently authenticated players to rejoin without typing the password again.

```yaml
sessions:
  enabled: true
  duration-minutes: 5
  bind-to-ip: true
  bind-to-uuid: true
  bind-to-client-fingerprint: false
```

Recommended production values:

- Keep `bind-to-ip: true` for public cracked servers.
- Keep session duration short.
- Use longer session times only if your player base expects it and you understand the tradeoff.

## Limbo

CrusAuth can move unauthenticated players into a safe limbo location while they log in.

```yaml
limbo:
  enabled: true
  world-name: "crusauth_limbo"
  create-world: true
  x: 0.5
  y: 80.0
  z: 0.5
  create-platform: true
  platform-material: "BARRIER"
  disable-gravity: true
  invulnerable: true
  keep-in-void: true
```

### Paper

On Paper, CrusAuth can create a dedicated void limbo world automatically when `create-world: true`.

### Folia

Folia does not support creating worlds at runtime in the same way Paper does. For Folia:

- pre-create and load the limbo world yourself, or
- use a fixed spawn instead of a dedicated generated limbo world.

Only enable Folia limbo if the world already exists and is loaded:

```yaml
limbo:
  folia-enabled: true
```

## Spawn behavior

Pre-auth behavior:

```yaml
spawn:
  pre-auth-mode: "limbo-or-spawn"
```

Supported modes:

| Mode | Behavior |
|---|---|
| `limbo-or-spawn` | Use limbo if available, otherwise fixed spawn if enabled. Recommended. |
| `limbo` | Use only limbo. |
| `spawn` | Use only configured fixed spawn. |
| `last-location` / `join-location` | Do not pre-teleport; restrict the player where they joined. |

Post-auth behavior:

```yaml
spawn:
  post-auth-mode: "last-location"
```

Supported modes:

| Mode | Behavior |
|---|---|
| `last-location` | Return to the captured gameplay location. |
| `spawn` | Send to the configured fixed spawn. |
| `proxy-target` | Use proxy forwarding instead of local teleport. |
| `none` | Do not teleport after authentication. |

## Fixed spawn commands

Set the fixed spawn from your current position:

```text
/crusauth setspawn
```

Remove it:

```text
/crusauth delspawn
```

## Restrictions before login

Default restricted actions include:

- chat;
- movement and rotation;
- inventory edits;
- item switching;
- block interaction;
- entity interaction;
- clicks;
- damage;
- most commands.

Allowed commands before login:

```yaml
restrictions:
  allowed-commands-before-login:
    - login
    - l
    - register
    - reg
```

Do not add economy, teleport, staff, or gameplay commands to this list.

## Messages

User-facing messages are stored in:

```text
plugins/CrusAuth/messages.yml
```

You can edit wording and colors there. Keep password-bearing command examples clear, especially `/login` and `/register`.
