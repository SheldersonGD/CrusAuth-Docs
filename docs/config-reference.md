# Configuration Reference

This page summarizes the main CrusAuth configuration sections. The generated config includes inline comments; keep those comments as the primary reference when upgrading.

## Backend license

```yaml
license:
  key: ""
```

Set your purchased CrusAuth license key here.

## Backend security

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
```

The bridge secret must match the proxy config when Velocity or BungeeCord is used.

## Standalone mode

```yaml
standalone:
  auto-login-online-mode-players: true
  offline-mode-name-lookup-premium-auto-login: false
```

Keep `offline-mode-name-lookup-premium-auto-login` disabled on public offline-mode servers. Username lookup alone does not prove ownership.

## Storage

```yaml
storage:
  type: sqlite
```

Supported values:

- `sqlite`
- `mysql`
- `mariadb`
- `jdbc`
- `yaml` for legacy/testing only

## Sessions

```yaml
sessions:
  enabled: true
  duration-minutes: 5
  bind-to-ip: true
  bind-to-uuid: true
  bind-to-client-fingerprint: false
```

## Limbo

```yaml
limbo:
  enabled: true
  world-name: "crusauth_limbo"
  create-world: true
  create-platform: true
  disable-gravity: true
  invulnerable: true
  keep-in-void: true
```

On Folia, pre-create the limbo world before enabling Folia limbo.

## Spawn

```yaml
spawn:
  pre-auth-mode: "limbo-or-spawn"
  post-auth-mode: "last-location"
  use-fixed-spawn: false
```

Use `/crusauth setspawn` to save a fixed spawn.

## Proxy forwarding

```yaml
proxy-forwarding:
  connect-after-auth: false
  target-server: "lobby"
  connect-delay-ticks: 10
```

Enable this when using a dedicated auth backend and transferring players to a lobby after authentication.

## Restrictions

```yaml
restrictions:
  allowed-commands-before-login:
    - login
    - l
    - register
    - reg
  cancel-chat: true
  cancel-movement: true
  cancel-rotation: true
  cancel-hotbar-switch: true
  cancel-inventory: true
  cancel-interaction: true
  cancel-clicks: true
  cancel-damage: true
```

Only allow commands that are required for authentication.

## Proxy config

Velocity and BungeeCord use the same main options:

```yaml
premium-check-enabled: true
force-online-for-premium: true
force-offline-for-cracked: true
fallback-to-offline-if-mojang-fails: true
lookup-timeout-ms: 2500
positive-cache-minutes: 1440
negative-cache-minutes: 10
bridge-secret: "PASTE_THE_SAME_LONG_RANDOM_SECRET_HERE"
bridge-message-delay-ms: 300

trusted-backend-servers:
  - auth
```

## Premium name policy

```yaml
premium-name-policy:
  require-client-uuid-match-for-auto-premium: true
  send-cracked-marker-to-backend: true
  protect-claimed-premium-names: true
  cracked-registered-account-wins: true
```

Keep these enabled for most hybrid networks.
