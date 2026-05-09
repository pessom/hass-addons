<!-- https://developers.home-assistant.io/docs/add-ons/presentation#keeping-a-changelog -->

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
