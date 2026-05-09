# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A Home Assistant add-on repository. Each subdirectory is a standalone add-on that can be installed via the HA add-on store by adding `https://github.com/pessom/hass-addons` as a custom repository.

## Add-ons

| Directory | Description | Supported arch |
|---|---|---|
| `clickhouse_hass` | ClickHouse DBMS for storing HA sensor/event history | amd64 only |
| `postfix_hass` | Postfix relay for outgoing email | all arches |

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
2. **`<svc>`** (longrun) — starts the actual daemon in foreground. The `finish` script halts the container on non-zero exit.

Scripts use `#!/command/with-contenv bashio` and the `bashio::` helper library for config access and logging.

## Building / testing

There is no local build toolchain. Add-ons are built by Home Assistant CI when pushed to GitHub. To test locally:

```bash
# Build a specific add-on image (replace <arch> with amd64, aarch64, etc.)
docker build \
  --build-arg BUILD_FROM=ghcr.io/home-assistant/amd64-base:3.19 \
  -t local/<addon>:test \
  ./<addon>
```

For `clickhouse_hass`, the Dockerfile uses a two-stage build pulling binaries from the official ClickHouse image, so `docker build` requires network access.

## Key conventions

- **Version bumps**: increment `version` in `config.yaml` and add an entry to `CHANGELOG.md` before every release.
- **Slug naming**: `config.yaml` `slug` must be globally unique across HA. The clickhouse addon currently uses `clickhouse_hass_testing` (note the `_testing` suffix) even in the stable directory.
- **Port mapping in clickhouse**: HTTP port is `8124` (not the default 8123) to avoid conflicts.
- **ClickHouse config** is generated at runtime by `init-clickhouse-server/run` using `xmlstarlet`. The master config lives at `/data/config/clickhouse-server/config.xml`; overrides go in `config.d/hass.xml`.
- **Postfix config**: `myhostname` and `mynetworks` are required options set via `bashio::config` in `init-postfix/run`. The queue directory is `/data/postfix`.
