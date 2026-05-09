<!-- https://developers.home-assistant.io/docs/add-ons/presentation#keeping-a-changelog -->

## 3.10.9-ha1

- Update base image to amd64-base:3.23 (Alpine 3.23)
- Update postfix 3.8.13 → 3.10.9
- Update cyrus-sasl 2.1.28-r5 → 2.1.28-r9
- Drop multi-arch support: amd64 only (aarch64, armhf, armv7, i386 removed)

## 3.8.13-ha3

- Fix: schema groups virtual/limits/restrictions were missing default values in options — caused "Missing option" error on config save

## 3.8.13-ha2

- Add aliases support: /config/postfix/aliases loaded on every startup
- On first start default aliases file is copied from image to /config/postfix/aliases
- Add config to map for File Editor access

## 3.8.13-ha1

- Update postfix 3.8.7 → 3.8.13 (latest in Alpine 3.19)
- Add postfix-pcre for regexp-based header_checks and body_checks
- Add cyrus-sasl, cyrus-sasl-login, cyrus-sasl-crammd5 for SASL authentication on inbound connections

## 3.8.7-ha2

- Add master.cf.tmpl: ports 587 (submission) and 465 (smtps) now activated via UI options
- Add ports.submission_enabled and ports.smtps_enabled config options
- Fix: ports 587/465 were declared in config.yaml but not served by postfix

## 3.8.7-ha1

- Versioning scheme changed to match bundled Postfix version (3.8.7)
- Full configuration via HA UI: general, network, TLS, SASL, restrictions, virtual domains, limits
- main.cf now generated from template on every startup — no more manual file editing
- TLS cert/key default to /ssl/fullchain.pem and /ssl/privkey.pem (HA shared SSL)
- SASL credentials written to /share/postfix/sasl_passwd automatically
- Add startup/boot for automatic start with Home Assistant
- Fix: added exec before postfix start-fg for correct s6 process tracking
- Fix: log_level now applied via bashio::log.level

## 0.0.4

- create config postfix in repo;
- remove syslog-ng; 
- syslog-ng config set ~defaul; 
- add config postfix; 
- edit startup syslog-ng; 
- fix problem on startup syslog-ng; 
- add syslog-ng;
- configuring syslog-ng; 

## 0.0.3-6

- change name addon;
- removing unused code; 

## 0.0.3-5

- edit script for init-postfix

## 0.0.3-4

- fix problem on start foreground postfix

## 0.0.3-3

- edit run script postfix

## 0.0.3-2

- edit start script for run postfix
- add dependency for init-postfix

## 0.0.3-1

- fix permissions for run

## 0.0.3

- fix structure for run

## 0.0.2

- Edit start command postfix

## 0.0.1

- Initial release