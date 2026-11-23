# ansible-pgbouncer

Installs and configures [PgBouncer](https://www.pgbouncer.org/) on Ubuntu (22.04, 24.04, 26.04).
The package comes from the apt repositories already configured on the host (e.g. apt.postgresql.org).

## Version

| Variable | Default | |
|---|---|---|
| `pgbouncer_version` | `""` | Upstream version, e.g. `"1.26.1"` (`pgbouncer=1.26.1-*`). Empty: whatever apt offers. |
| `pgbouncer_hold` | `true` | `apt-mark hold pgbouncer`, so `apt upgrade` does not move it. |
| `pgbouncer_allow_upgrade` | `false` | An installed version other than `pgbouncer_version` fails the play unless this is `true`. |
| `pgbouncer_extra_packages` | `[]` | Installed next to pgbouncer. |

## Configuration

`pgbouncer.ini` and `pg_auth` are rendered from `postgresql_db_users` (items with `pgbouncer` defined: `role`, `dbname`,
`passwd`) and `pg_database_data` (`host`, `port`). Optional overrides: `pgbouncer_port`, `pgbouncer_listen_addr`,
`pgbouncer_config_path`, `pgbouncer_pg_auth`, `pgbouncer_logfile`, `pgbouncer_pidfile`, `pgbouncer_unix_socket_dir`,
`pgbouncer_daemon`.

- A change of `pgbouncer.ini` restarts pgbouncer (client connections are dropped).
- A change of `pg_auth` reloads it (connections are kept).
- `pgbouncer.ini` contains passwords: use `diff: false` on the role (or no `--diff`).

## Example

```yaml
- hosts: postgres
  become: true
  roles:
    - role: pastir.pgbouncer
      diff: false
      vars:
        pgbouncer_version: "1.26.1"
        pgbouncer_port: 6432
        pg_database_data: { host: 127.0.0.1, port: 5432 }
        postgresql_db_users:
          - { role: appuser, dbname: app, passwd: "{{ app_db_password }}", pgbouncer: true }
```
