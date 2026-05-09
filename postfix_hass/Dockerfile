ARG BUILD_FROM
FROM $BUILD_FROM

COPY rootfs /

RUN apk add --no-cache \
    postfix=3.10.9-r0 \
    postfix-pcre=3.10.9-r0 \
    cyrus-sasl=2.1.28-r9 \
    cyrus-sasl-login=2.1.28-r9 \
    cyrus-sasl-crammd5=2.1.28-r9 && \
    mkdir -p /usr/share/doc/postfix/readme
