# FAQ

## Does CrusAuth support premium and cracked players together?

Yes. Use the Paper/Folia core with either the Velocity or BungeeCord addon for hybrid premium/cracked networks.

## Can I use CrusAuth without a proxy?

Yes, but only as a standalone cracked/offline authentication plugin. Secure premium auto-login requires a proxy because the proxy must perform the premium/cracked decision before backend gameplay.

## Is SQLite enough?

For a single auth backend, yes. SQLite is the default and creates `plugins/CrusAuth/accounts.db`.

Use MySQL/MariaDB if multiple auth backends need shared account storage.

## Do I need both Velocity and BungeeCord addons?

No. Buy and install only the addon for your proxy platform.

## What happens if a cracked player uses a premium username?

If the premium owner has already claimed the name, the cracked client is blocked before backend gameplay. If the cracked account registered first and the cracked-first policy is enabled, the account remains password-protected and the premium player must log in.

## Why does the proxy addon say it is waiting for Paper authorization?

The Paper/Folia core validates the license and sends signed addon status to the proxy. Make sure the backend is online, the license key is valid, the bridge secret matches, and the auth backend is listed in `trusted-backend-servers`.

## Can I change messages?

Yes. Edit:

```text
plugins/CrusAuth/messages.yml
```

## Can players delete their own accounts?

The `/unregister` command exists, but its permission is disabled by default. Grant `crusauth.command.unregister` only if your server policy allows self-deletion.

## Should I publish my config when asking for support?

Only after redacting secrets. Remove license keys, bridge secrets, database passwords, IPs, and server identifiers.
