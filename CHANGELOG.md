# Changelog

## 2.0.0

### Breaking
- `pgbouncer_packages` is no longer used: the role installs `pgbouncer` itself;
  additional packages go to `pgbouncer_extra_packages`.

### Added
- `pgbouncer_version` (exact upstream version), `pgbouncer_hold` (apt hold, default on),
  `pgbouncer_allow_upgrade` (guard against an implicit version change, default off).
- `reload pgbouncer` handler.

### Changed
- A change of `pgbouncer.ini` now restarts pgbouncer (previously only `pg_auth` did, so ini changes were not applied).
- A change of `pg_auth` reloads pgbouncer instead of restarting it (auth_file is re-read on reload).
- Supported platforms: Ubuntu jammy, noble, resolute.
