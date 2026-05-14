# Premium & Cracked Account Behavior

CrusAuth is designed for hybrid networks where premium and cracked players may both connect, but account ownership must remain predictable and safe.

The Paper/Folia core stores account state. Velocity or BungeeCord performs the proxy-side premium/cracked decision when a proxy addon is installed.

## Standalone behavior

On a standalone Paper/Folia server without a proxy:

- cracked players use `/register` and `/login`;
- unauthenticated players are restricted until login/register succeeds;
- sessions can keep recently authenticated players logged in for a short time;
- the backend cannot securely prove premium ownership while running offline-mode.

For secure premium auto-login, use Velocity or BungeeCord.

## Hybrid proxy behavior

With a proxy addon installed:

1. The proxy receives the connection first.
2. CrusAuth checks the username and premium lookup state.
3. The proxy decides whether to force online-mode or offline-mode for that connection.
4. The proxy sends a signed bridge message to the backend.
5. The Paper/Folia core accepts the bridge message only if it is signed with the shared secret and is not a replay.
6. Premium users can be auto-authenticated.
7. Cracked users must register or log in.

## Account types

CrusAuth separates account ownership into two practical states:

| Account type | Meaning |
|---|---|
| Premium | A real Mojang/Microsoft-authenticated owner has claimed this visible username. |
| Cracked | A password account exists for this visible username. |

The system avoids silently converting cracked accounts into premium accounts unless explicitly configured otherwise.

## Premium-first case

When a real premium owner joins first:

1. The proxy verifies the premium identity.
2. The backend stores the premium account state.
3. The username becomes premium-claimed.
4. Later cracked launchers using the same visible name are forced through official premium authentication.
5. If they do not own that account, they fail before backend gameplay.

This protects premium users from cracked impersonation.

## Cracked-first case

When a cracked user registers a username first:

1. The cracked/password account owns that visible username in CrusAuth.
2. A later premium user using the same visible name does not silently take over the account.
3. The premium user must use `/login` if the cracked-first policy is enabled.

Relevant setting:

```yaml
premium-name-policy:
  cracked-registered-account-wins: true
```

Backend-side protection:

```yaml
security:
  cracked-registered-account-wins-over-auto-premium: true
```

This is important for networks migrating from older offline-mode systems.

## Claimed premium name protection

Proxy setting:

```yaml
premium-name-policy:
  protect-claimed-premium-names: true
```

Backend setting:

```yaml
security:
  block-cracked-login-when-premium-account-exists: true
```

When enabled, a cracked launcher cannot use a username that has already been claimed by the real premium owner.

## UUID match requirement

Proxy setting:

```yaml
premium-name-policy:
  require-client-uuid-match-for-auto-premium: true
```

This prevents a common hybrid-auth problem: a cracked launcher can use a visible name that exists as a premium profile, but that does not prove the player owns that premium account.

With this enabled, CrusAuth does not auto-premium a user just because the username exists. The connecting client must match the expected premium identity in the proxy login flow.

## Mojang lookup fallback

Proxy setting:

```yaml
fallback-to-offline-if-mojang-fails: true
```

When enabled, if the premium lookup service is temporarily unavailable, CrusAuth can keep the player cracked-compatible instead of denying the connection.

This is safer for uptime, but it means premium auto-login may not happen during lookup outages.

If you prefer strict security over availability, set it to false:

```yaml
fallback-to-offline-if-mojang-fails: false
```

## Recommended production policy

Proxy:

```yaml
premium-name-policy:
  require-client-uuid-match-for-auto-premium: true
  send-cracked-marker-to-backend: true
  protect-claimed-premium-names: true
  cracked-registered-account-wins: true
```

Backend:

```yaml
security:
  allow-premium-upgrade-of-existing-cracked-account: false
  block-cracked-login-when-premium-account-exists: true
  cracked-registered-account-wins-over-auto-premium: true
```

These defaults are conservative and protect both premium owners and existing cracked/password accounts.
