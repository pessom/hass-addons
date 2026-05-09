<!-- https://developers.home-assistant.io/docs/add-ons/presentation#keeping-a-changelog -->

## 3.13.7-ha1

- Versioning scheme changed to match bundled RabbitMQ version (3.13.7)
- Add startup/boot for automatic start with Home Assistant
- Fix: s6 type/up files missing newline
- Fix: rabbitmq/run and rabbitmq/finish not executable
- Fix: unreachable log line after exec in rabbitmq/run
- Fix: init script config copy logic broken for subsequent starts
- Fix: replace hardcoded sleep 15s with rabbitmqctl await_startup loop (60s timeout)
- Fix: log_level from HA config now applied to rabbitmq.conf
- Fix: log_level schema expanded (was only info|error)

## 0.0.1

- Run rabbitmq in a Home Assistant add-on
