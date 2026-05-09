<!-- https://developers.home-assistant.io/docs/add-ons/presentation#keeping-a-changelog -->

## 25.1.2.3-ha6

- Update base image to amd64-base:3.23 (Alpine 3.23)

## 25.1.2.3-ha5

- Fix: schema groups performance/storage were missing default values in options — caused "Missing option" error on config save

## 25.1.2.3-ha4

- Fix: s6 type files missing newline (longrun, oneshot)
- Fix: run and finish scripts not executable
- Fix: unreachable log line after exec in clickhouse-server/run

## 25.1.2.3-ha3

- Add logging config: level, log_queries
- Add performance config: max_connections, max_concurrent_queries, max_server_memory_usage, max_server_memory_usage_to_ram_ratio, background_pool_size
- Add security config: default_password (stored as SHA256 in separate config file)
- Add storage config: max_table_size_to_drop, max_partition_size_to_drop
- All new parameters optional — unset means ClickHouse uses its own defaults

## 25.1.2.3-ha2

- Ports configurable via HA UI: http_port, tcp_port, mysql_port, postgresql_port
- Unset ports are not added to config — ClickHouse won't listen on them
- Fix typos in ports_description (mysq_port -> MySQL, tcp -> TCP native)

## 25.1.2.3-ha1

- Versioning scheme changed to match bundled ClickHouse version (25.1.2.3)
- Fix slug: clickhouse_hass_testing → clickhouse_hass
- Fix url: updated to correct repository path
- Add startup/boot for automatic start with Home Assistant
- Remove unused map: share
- Fix: wrong comment header in clickhouse-server/run (was "Init postfix")

## 0.0.1

- Run clickhouse-server in a Home Assistant add-on
