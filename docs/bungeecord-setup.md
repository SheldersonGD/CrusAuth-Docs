# BungeeCord Setup

The BungeeCord module is an optional paid addon for hybrid premium/cracked networks that use BungeeCord or a compatible Bungee fork.

Use this module if your network uses BungeeCord instead of Velocity.

## What the BungeeCord module does

- Checks whether a username exists as a Mojang premium profile.
- Forces premium authentication for verified premium players when supported by the proxy implementation.
- Keeps cracked-compatible players in offline mode.
- Protects premium-claimed usernames from cracked impersonation.
- Sends signed bridge messages to the Paper/Folia backend.
- Blocks unauthenticated players from switching to non-auth backend servers.

## Installation

1. Stop BungeeCord.
2. Put `CrusAuth-Bungee-x.x.x.jar` into the BungeeCord `plugins/` folder.
3. Start BungeeCord once to generate the config.
4. Stop BungeeCord.
5. Edit the generated CrusAuth Bungee config.
6. Set the same `bridge-secret` used in the Paper/Folia config.
7. Configure `trusted-backend-servers`.
8. Start BungeeCord and the Paper/Folia backend.

## Basic config

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

permissions:
  proxy-admin: "crusauth.proxy.admin"
```

`trusted-backend-servers` must contain the exact BungeeCord server name of the auth/lobby backend where the Paper/Folia core is installed.

## Premium name policy

```yaml
premium-name-policy:
  require-client-uuid-match-for-auto-premium: true
  send-cracked-marker-to-backend: true
  protect-claimed-premium-names: true
  cracked-registered-account-wins: true
```

Keep these values enabled for most hybrid networks.

## Important Bungee compatibility note

BungeeCord does not expose the same clean per-connection online/offline API as Velocity. CrusAuth uses the common internal Bungee connection method used by many Bungee-compatible forks.

If your Bungee fork blocks or removes that method, CrusAuth will fail closed for protected premium-name cases instead of letting a cracked client reach the backend under a claimed premium username.

If you need the most predictable hybrid premium/cracked behavior, Velocity is recommended.

## Proxy command

```text
/crusauthproxy reload
```

Alias:

```text
/caproxy reload
```

Default permission:

```text
crusauth.proxy.admin
```

The permission name can be changed in the Bungee module config.

## Addon license behavior

The BungeeCord module requires a CrusAuth license that includes the BungeeCord addon. The Paper/Folia core validates the license and sends signed addon status to the proxy through the secure bridge.

If the license does not include BungeeCord, the proxy module will deny hybrid functionality and show an addon-not-enabled message.

## Security notes

- Do not expose backend servers directly to players.
- Use one long random bridge secret shared only between the proxy and backend.
- Keep the auth backend in the trusted backend list.
- Do not put game servers in `trusted-backend-servers` unless they run the Paper/Folia core and are intentionally part of the auth flow.
- Test both premium and cracked login flows before opening the network.
