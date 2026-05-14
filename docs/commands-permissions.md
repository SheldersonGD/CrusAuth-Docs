# Commands and Permissions

CrusAuth includes player commands, backend admin commands, and proxy admin commands.

## Player commands

| Command | Description | Permission | Default |
|---|---|---|---|
| `/login <password>` | Log in to an existing cracked/password account. | `crusauth.command.login` | true |
| `/register <password> <password>` | Register a new cracked/password account. | `crusauth.command.register` | true |
| `/logout` | Log out from the current session. | `crusauth.command.logout` | false |
| `/changepassword <oldPassword> <newPassword>` | Change a cracked account password. | `crusauth.command.changepassword` | true |
| `/unregister <password>` | Delete your own cracked account. | `crusauth.command.unregister` | false |

Aliases:

| Command | Alias |
|---|---|
| `/login` | `/l` |
| `/register` | `/reg` |
| `/changepassword` | `/changepw` |

Premium accounts cannot use password-only account commands such as `/changepassword` and `/unregister`.

## User permission group

```text
crusauth.user
```

Grants normal player authentication commands:

- `crusauth.command.login`
- `crusauth.command.register`
- `crusauth.command.changepassword`

## Backend admin command

Main command:

```text
/crusauth
```

| Command | Description | Permission |
|---|---|---|
| `/crusauth help` | Show admin command help. | `crusauth.admin.help` |
| `/crusauth reload` | Reload config, messages, and storage. | `crusauth.admin.reload` |
| `/crusauth stats` | Show account/session/storage stats. | `crusauth.admin.stats` |
| `/crusauth setspawn` | Save the fixed auth/post-auth spawn. | `crusauth.admin.setspawn` |
| `/crusauth delspawn` | Remove the fixed spawn. | `crusauth.admin.delspawn` |
| `/crusauth clearspawn` | Alias for `/crusauth delspawn`. | `crusauth.admin.delspawn` |
| `/crusauth info <player>` | Show account state for a username. | `crusauth.admin.info` |
| `/crusauth register <player> <password>` | Create a cracked password account. | `crusauth.admin.register` |
| `/crusauth changepassword <player> <newPassword> [cracked\|premium]` | Reset a stored password. | `crusauth.admin.changepassword` |
| `/crusauth unregister <player> [all\|cracked\|premium]` | Delete stored account data. | `crusauth.admin.unregister` |
| `/crusauth forcelogin <player>` | Force-authenticate an online cracked player. | `crusauth.admin.forcelogin` |
| `/crusauth forcelogout <player>` | Return an online player to auth state. | `crusauth.admin.forcelogout` |
| `/crusauth resetattempts <player>` | Clear temporary failed-login lockout. | `crusauth.admin.resetattempts` |

Parent permission:

```text
crusauth.admin
```

Default: operator.

## Proxy command

Velocity and BungeeCord modules provide:

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

The permission can be changed in the proxy module config:

```yaml
permissions:
  proxy-admin: "crusauth.proxy.admin"
```

## Bypass permission

```text
crusauth.bypass
```

This allows bypassing authentication. It is disabled by default and should not be granted to normal players.

Use it only for controlled staff/debug cases. A compromised account with auth bypass can skip important security checks.

## Recommended LuckPerms examples

Normal players usually do not need manual permissions because login/register permissions are enabled by default.

Grant admin access:

```text
/lp user <name> permission set crusauth.admin true
```

Grant proxy admin access:

```text
/lp user <name> permission set crusauth.proxy.admin true
```

Enable `/logout` for players:

```text
/lp group default permission set crusauth.command.logout true
```

Enable `/unregister` for players:

```text
/lp group default permission set crusauth.command.unregister true
```

Only enable unregister if your server policy allows players to delete their own auth accounts.
