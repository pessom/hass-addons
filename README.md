# Home Assistant Addons by pessom

## Installation

Add this repository to Home Assistant:

```
https://github.com/pessom/hass-addons
```

Navigate to **Settings → Add-ons → Add-on Store → ⋮ → Repositories**, paste the URL and click Add.

---

## Addons

### ClickHouse

High-performance column-oriented DBMS for storing and analyzing Home Assistant sensor history, events and automations.

**Supported architectures:** amd64

**Ports:**
| Port | Protocol | Description |
|------|----------|-------------|
| 8124 | HTTP | ClickHouse HTTP interface |
| 9000 | TCP | ClickHouse native protocol |
| 9004 | TCP | MySQL protocol (optional) |
| 9005 | TCP | PostgreSQL protocol (optional) |

**Key configuration options:**
- `http_port` / `tcp_port` / `mysql_port` / `postgresql_port` — configurable, optional ports
- `logging.level` — log level (trace/debug/information/warning/error)
- `logging.log_queries` — log all queries
- `performance.max_server_memory_usage` — RAM limit in bytes
- `performance.max_concurrent_queries` — max parallel queries
- `security.default_password` — password for the `default` user (stored as SHA256)
- `storage.max_table_size_to_drop` — protection against accidental DROP of large tables

Data is persisted in `/data/clickhouse/` and survives addon restarts and updates.

---

### Postfix

Full-featured SMTP server with support for relay, TLS and SASL authentication.

**Supported architectures:** aarch64, amd64, armhf, armv7, i386

**Ports:**
| Port | Protocol | Description |
|------|----------|-------------|
| 25 | TCP | SMTP |
| 587 | TCP | SMTP Submission (optional) |
| 465 | TCP | SMTPS (optional) |

**Key configuration options:**
- `general.hostname` / `general.domain` / `general.origin` — server identity
- `network.relayhost` — upstream SMTP relay (e.g. `[smtp.gmail.com]:587`)
- `network.mynetworks` — trusted networks allowed to relay
- `tls.smtpd_level` / `tls.smtp_level` — TLS security level (none/may/encrypt)
- TLS certificates default to `/ssl/fullchain.pem` and `/ssl/privkey.pem` (shared HA SSL)
- `sasl.smtp_enabled` — SASL auth for outbound relay
- `sasl.smtpd_enabled` — SASL auth for inbound connections
- `ports.submission_enabled` / `ports.smtps_enabled` — enable ports 587/465

Mail aliases are stored in `/config/postfix/aliases` (editable via File Editor addon). A default file is created on first start.

---

### RTL-SDR TCP Server

Exposes a physically connected RTL-SDR dongle over the network via `rtl_tcp`, allowing SDR clients (SDR#, GQRX, SDR++) to connect remotely.

**Supported architectures:** amd64

**Ports:**
| Port | Protocol | Description |
|------|----------|-------------|
| 1234 | TCP | rtl_tcp server (configurable) |

**Key configuration options:**
- `port` — listening port (default: 1234)
- `gain` — tuner gain in dB (0 = AGC auto)
- `ppm` — frequency correction in PPM
- `device_index` — device index or serial number (useful when multiple dongles connected)
- `sample_rate` — sample rate in Hz (e.g. 2048000)
- `bias_t` — enable Bias-T power on antenna port (rtl-sdr.com v3/v4)
- `direct_sampling` — enable direct HF sampling (reception below 24 MHz)

Unset optional parameters are not passed to `rtl_tcp` — the application uses its own defaults.

Connect your SDR client to `<home-assistant-ip>:1234`.

---

## License

MIT License
