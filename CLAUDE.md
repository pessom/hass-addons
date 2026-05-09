# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A Home Assistant add-on repository. Each subdirectory is a standalone add-on that can be installed via the HA add-on store by adding `https://github.com/pessom/hass-addons` as a custom repository.

## Add-ons

| Directory | Description | Supported arch |
|---|---|---|
| `clickhouse_hass` | ClickHouse DBMS for storing HA sensor/event history | amd64 only |
| `postfix_hass` | Postfix SMTP server with TLS and SASL support | all arches |
| `rabbitmq_hass` | RabbitMQ messaging broker | amd64 only |
| `rtlsdr_hass` | RTL-SDR dongle exposed via rtl_tcp | amd64 only |

## Versioning

Version scheme: `<bundled_app_version>-ha<revision>`. Examples: `3.8.13-ha1`, `25.1.2.3-ha4`.

- Bump revision (`-ha1` → `-ha2`) for addon-only changes (scripts, config)
- Reset revision to `-ha1` when bundled app version changes
- **Always bump version** in `config.yaml` and add entry to `CHANGELOG.md` with every change

## Add-on structure

Each add-on follows the same layout:

```
<addon>/
  config.yaml       # HA metadata: slug, version, ports, options schema
  build.yaml        # Base images per arch (ghcr.io/home-assistant/<arch>-base:3.19)
  Dockerfile        # Builds on top of BUILD_FROM base image; copies rootfs/
  CHANGELOG.md
  rootfs/           # Overlaid onto the container filesystem
    etc/
      s6-overlay/   # s6-overlay v3 service supervision
        s6-rc.d/
          init-<svc>/   # oneshot: runs once on startup, sets up config/dirs
          <svc>/        # longrun: runs the daemon itself
```

### s6-overlay service pattern

Every add-on uses two s6 services chained via `dependencies.d/`:

1. **`init-<svc>`** (oneshot) — reads `bashio::config` values, creates data dirs, generates config files. Data is always persisted under `/data/` (survives container restarts).
2. **`<svc>`** (longrun) — starts the actual daemon in foreground via `exec`. The `finish` script halts the container on non-zero exit.

Scripts use `#!/command/with-contenv bashio` and the `bashio::` helper library for config access and logging.

**Critical s6 requirements:**
- `type` and `up` files must end with a newline (`\n`)
- `run` and `finish` scripts must be executable (`chmod +x`)
- Always use `exec` before the daemon command in longrun `run` scripts
- `init: false` must be set in `config.yaml` for s6-overlay v3

### Optional config parameters

Parameters that should not be passed to the app when unset use `?` suffix in schema:

```yaml
schema:
  gain: float?   # not passed to app if empty
```

In init scripts, check with `bashio::config.has_value` before using:

```bash
if bashio::config.has_value 'gain'; then
    ARGS="${ARGS} -g $(bashio::config 'gain')"
fi
```

### First-run file provisioning pattern

For user-editable config files (e.g. postfix aliases):
- Ship a default file in `rootfs/etc/<app>/`
- On first start: if file absent in `/config/<app>/` → copy from image
- On every start: always use the file from `/config/<app>/`

## Building / testing

There is no local build toolchain. Add-ons are built by Home Assistant CI when pushed to GitHub. To test locally:

```bash
docker build \
  --build-arg BUILD_FROM=ghcr.io/home-assistant/amd64-base:3.19 \
  -t local/<addon>:test \
  ./<addon>
```

## Key conventions

- **`startup: services`** and **`boot: auto`** — required for all addons for automatic start with HA
- **`usb: true`** — required for addons accessing USB devices (rtlsdr_hass)
- **Port descriptions** use title case: `"HTTP port"`, `"TCP (native) port"`, not `"http port"`
- **ClickHouse config** is regenerated from scratch on every start into `config.d/hass.xml` — never use `xmlstarlet -s` (causes duplicates on restart)
- **Postfix config** is generated from `main.cf.tmpl` and `master.cf.tmpl` on every start
- **TLS certificates** default to `/ssl/fullchain.pem` and `/ssl/privkey.pem` (HA shared SSL via `map: ssl`)
- **SASL credentials** for postfix written to `/share/postfix/sasl_passwd` (via `map: share`)
- **Mail aliases** stored in `/config/postfix/aliases` (via `map: config`), editable via File Editor addon
