# Velocity Setup

The Velocity module is an optional paid addon for hybrid premium/cracked networks. It runs on the proxy and decides whether a connecting username should be treated as premium or cracked before the player reaches backend gameplay.

Use this module if your network uses Velocity.

## What the Velocity module does

- Checks whether a username exists as a Mojang premium profile.
- Forces official online authentication for verified premium players when appropriate.
- Keeps cracked-compatible players in offline mode.
- Protects premium-claimed usernames from cracked impersonation.
- Sends signed bridge messages to the Paper/Folia backend.
- Blocks unauthenticated players from switching to non-auth backend servers.

## Installation

1. Stop Velocity.
2. Put `CrusAuth-Velocity-x.x.x.jar` into the Velocity `plugins/` folder.
3. Start Velocity once to generate the config.
4. Stop Velocity.
5. Edit the generated CrusAuth Velocity config.
6. Set the same `bridge-secret` used in the Paper/Folia config.
7. Configure `trusted-backend-servers`.
8. Start Velocity and the Paper/Folia backend.

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

`trusted-backend-servers` must contain the exact Velocity server name of the auth/lobby backend where the Paper/Folia core is installed.

## Premium name policy

```yaml
premium-name-policy:
  require-client-uuid-match-for-auto-premium: true
  send-cracked-marker-to-backend: true
  protect-claimed-premium-names: true
  cracked-registered-account-wins: true
```

Recommended production values are shown above.

### `require-client-uuid-match-for-auto-premium`

When enabled, CrusAuth only treats a premium-looking username as premium auto-login if the connecting client UUID matches the Mojang UUID expected for that username.

This prevents unclaimed premium-looking names from being incorrectly kicked or forced through premium auth just because the name exists on Mojang's side.

### `protect-claimed-premium-names`

When enabled, once a real premium owner has claimed a username, cracked launchers using that same visible username are forced through official Mojang authentication and fail before reaching backend gameplay.

### `cracked-registered-account-wins`

When enabled, if a cracked/password account registered the username first, a later premium owner using the same visible username must log in normally and cannot silently take over the account.

This protects existing cracked users during hybrid transitions.

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

The permission name can be changed in the Velocity module config.

## Addon license behavior

The Velocity module requires a CrusAuth license that includes the Velocity addon. The Paper/Folia core validates the license and sends signed addon status to the proxy through the secure bridge.

If the license does not include Velocity, the proxy module will deny hybrid functionality and show an addon-not-enabled message.

## Security notes

- Do not expose backend servers directly to players.
- Use one long random bridge secret shared only between the proxy and backend.
- Keep the auth backend in the trusted backend list.
- Do not put game servers in `trusted-backend-servers` unless they run the Paper/Folia core and are intentionally part of the auth flow.
- Test premium and cracked behavior before opening the network.
