# Storage: SQLite, MySQL and MariaDB

CrusAuth uses SQLite by default. This keeps first installation simple and avoids unnecessary database connections on small servers.

For larger networks, MySQL or MariaDB can be enabled manually.

## Default SQLite mode

Default config:

```yaml
storage:
  type: sqlite
```

SQLite creates a local database file:

```text
plugins/CrusAuth/accounts.db
```

Use SQLite when:

- you run one auth/lobby backend;
- you want a simple setup;
- you do not need shared account storage between multiple backend servers.

Back up `accounts.db` regularly.

## MySQL/MariaDB mode

Use MySQL or MariaDB when:

- multiple auth backends must share accounts;
- you run a larger network;
- you want database-level backups, replication, or monitoring.

Example:

```yaml
storage:
  type: mysql
  mysql:
    host: "127.0.0.1"
    port: 3306
    database: "crusauth"
    username: "crusauth"
    password: "strong_password_here"
```

For MariaDB, use:

```yaml
storage:
  type: mariadb
```

The same `mysql:` connection block is used.

## Create the database

Run this in MySQL/MariaDB before enabling remote storage:

```sql
CREATE DATABASE crusauth CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'crusauth'@'%' IDENTIFIED BY 'strong_password_here';
GRANT ALL PRIVILEGES ON crusauth.* TO 'crusauth'@'%';
FLUSH PRIVILEGES;
```

Use a strong unique password. Do not reuse the example password.

## Pool settings

CrusAuth uses HikariCP for pooled SQL connections.

Default pool block:

```yaml
storage:
  pool:
    maximum-pool-size: 10
    minimum-idle: 2
    connection-timeout-ms: 3000
    validation-timeout-ms: 3000
    idle-timeout-ms: 600000
    max-lifetime-ms: 1800000
    keepalive-time-ms: 300000
```

Guidelines:

- One auth backend: `maximum-pool-size: 8-12` is usually enough.
- Multiple auth backends: lower the pool size per backend.
- Do not set a very high pool size unless your database is tuned for it.
- If MySQL has a low connection limit, keep CrusAuth pools conservative.

## SQL fallback behavior

If MySQL/MariaDB/JDBC is enabled but unreachable, CrusAuth can fall back to SQLite:

```yaml
storage:
  fallback-to-sqlite-on-sql-failure: true
  log-sql-failure-stacktraces: false
```

This prevents a misconfigured database from blocking all authentication at startup.

For production, monitor logs after startup. If CrusAuth falls back to SQLite unexpectedly, fix the database connection before accepting real traffic.

## Migration from local storage

CrusAuth can import local account data into an empty SQL database on first run.

```yaml
storage:
  migration:
    import-local-on-first-run: true
    import-yaml: true
    import-sqlite: true
    sqlite-file: "accounts.db"
```

Before migrating:

1. Stop the server.
2. Back up `plugins/CrusAuth/accounts.db`.
3. Back up `plugins/CrusAuth/accounts.yml` if it exists.
4. Create an empty MySQL/MariaDB database.
5. Configure SQL storage.
6. Start the server and verify import logs.
7. Test login with existing accounts.

Do not run migration blindly on a live network.

## Advanced JDBC mode

Advanced users can provide a custom JDBC connection:

```yaml
storage:
  type: jdbc
  jdbc:
    driver-class: ""
    url: "jdbc:sqlite:plugins/CrusAuth/accounts.db"
    username: ""
    password: ""
    dialect: "auto"
```

Most servers should use `sqlite`, `mysql`, or `mariadb` instead.

## Files to keep private

Never upload or post these files publicly:

```text
plugins/CrusAuth/accounts.db
plugins/CrusAuth/accounts.yml
plugins/CrusAuth/config.yml
plugins/CrusAuth/license-cache.properties
plugins/CrusAuth/offline-license-slots.properties
plugins/CrusAuth/server-id.txt
plugins/CrusAuth/admin-audit.log
```

These files may contain account data, license state, server identifiers, private configuration, or administrative history.
