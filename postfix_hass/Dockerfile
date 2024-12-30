ARG BUILD_FROM
FROM $BUILD_FROM

COPY rootfs /

# Install postfix
RUN apk add --no-cache \
    postfix=3.8.7-r0 \
    && rm -rf /var/cache/apk/*
