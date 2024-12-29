ARG BUILD_FROM
FROM $BUILD_FROM

COPY rootfs /

# prepare for syslog-ng
RUN rm -rf /dev/log

# Install postfix
RUN apk add --no-cache \
    postfix=3.8.7-r0 \
    syslog-ng=4.5.0-r0
