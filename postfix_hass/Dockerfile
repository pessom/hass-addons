ARG BUILD_FROM
FROM $BUILD_FROM

# Install postfix
RUN apk add --no-cache \
    postfix=3.8.7-r0

COPY rootfs /