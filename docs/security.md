# Security Notes

Authentication plugins sit at the boundary between a player and the rest of the server. Treat CrusAuth configuration as security-sensitive.

## What CrusAuth protects

CrusAuth is designed to protect:

- cracked account registration and login;
- password storage;
- unauthenticated player movement and interaction;
- proxy-to-backend premium/cracked decisions;
- premium-claimed usernames;
- backend session state;
- basic brute-force pressure through attempt limits and rate limits.

## Password storage

CrusAuth does not store plaintext passwords.

Passwords are stored using PBKDF2-HMAC-SHA256 with random salts. The default iteration count is configured here:

```yaml
security:
  password-hash-iterations: 210000
```

Do not lower this value unless you understand the security and CPU tradeoff.

## Sensitive command logs

Password-bearing commands should not be written to normal server command logs.

Recommended:

```yaml
security:
  suppress-sensitive-command-logs: true
```

Still treat console logs carefully. Never post full startup logs publicly without checking for secrets first.

## Bridge security

The proxy addon and backend plugin communicate over a signed bridge.

The bridge uses:

- a shared secret;
- signed payloads;
- timestamp validation;
- nonce replay protection.

Backend config:

```yaml
security:
  bridge-secret: "PASTE_LONG_RANDOM_SECRET_HERE"
  bridge-message-max-age-ms: 10000
```

Proxy config:

```yaml
bridge-secret: "PASTE_LONG_RANDOM_SECRET_HERE"
```

Use the same secret on both sides. Do not reuse a public example value.

## Backend firewalling

For proxy networks, backend servers must not be directly reachable from the public internet.

Recommended:

- expose only the proxy port publicly;
- firewall backend ports;
- allow backend connections only from the proxy machine/IP;
- do not publish backend IPs;
- do not allow direct joins around the proxy.

If players can connect directly to the backend, they can bypass the proxy-side decision layer.

## Auth restrictions

Before authentication, CrusAuth can block:

- movement;
- rotation;
- chat;
- inventory actions;
- item switching;
- clicks;
- block interaction;
- entity interaction;
- damage;
- non-auth commands.

Only keep essential commands in `allowed-commands-before-login`.

## Files that must stay private

Do not share these files publicly:

```text
plugins/CrusAuth/config.yml
plugins/CrusAuth/accounts.db
plugins/CrusAuth/accounts.yml
plugins/CrusAuth/license-cache.properties
plugins/CrusAuth/offline-license-slots.properties
plugins/CrusAuth/server-id.txt
plugins/CrusAuth/account-index.properties
plugins/CrusAuth/admin-audit.log
```

Redact:

- license keys;
- bridge secrets;
- database passwords;
- IP addresses;
- server identifiers;
- player account data;
- private backend addresses.

## Safe support log policy

When sending logs for support:

1. Remove license keys.
2. Remove bridge secrets.
3. Remove database passwords.
4. Remove public IPs if you do not want them visible.
5. Keep stack traces and platform versions.
6. Keep the error lines around the issue.

Do not send your entire server folder.

## Security reports

If you believe you found a security issue, do not post exploit details publicly.

Send a private report through the official support channel with:

- affected CrusAuth version;
- platform and version;
- whether Velocity/Bungee is used;
- exact reproduction steps;
- logs with secrets removed;
- expected result and actual result.
