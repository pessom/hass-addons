ARG BUILD_FROM

FROM clickhouse:24.8 AS clickhouse-donor

FROM $BUILD_FROM

ARG REPOSITORY="https://packages.clickhouse.com/tgz/stable"
ARG VERSION="25.1.2.3"
ARG PACKAGES="clickhouse-client clickhouse-server clickhouse-common-static"
# optionals package, not needed now - clickhouse-odbc-bridge, clickhouse-library-bridge
ARG DEFAULT_CLIENT_CONFIG_DIR="/data/config/clickhouse-client/"
ARG DEFAULT_SERVER_CONFIG_DIR="/data/config/clickhouse-server/"
ARG DEFAULT_DATA_DIR="/data/clickhouse/data/"
ARG DEFAULT_LOG_DIR="/var/log/clickhouse-server"
ARG CLICKHOUSE_CONFIG="/data/config/clickhouse-server/config.xml"

ENV LANG=en_US.UTF-8 \
    LANGUAGE=en_US:en \
    LC_ALL=en_US.UTF-8 \
    TZ=UTC \
    CLICKHOUSE_CONFIG=/data/config/clickhouse-server/config.xml

COPY --from=clickhouse-donor /lib/x86_64-linux-gnu/libc.so.6 /lib/x86_64-linux-gnu/libdl.so.2 /lib/x86_64-linux-gnu/libm.so.6 /lib/x86_64-linux-gnu/libpthread.so.0 /lib/x86_64-linux-gnu/librt.so.1 /lib/x86_64-linux-gnu/libnss_dns.so.2 /lib/x86_64-linux-gnu/libnss_files.so.2 /lib/x86_64-linux-gnu/libresolv.so.2 /lib/x86_64-linux-gnu/ld-2.31.so /lib/x86_64-linux-gnu/
COPY --from=clickhouse-donor /etc/nsswitch.conf /etc/

RUN mkdir -p /lib64 && ln -sf /lib/x86_64-linux-gnu/ld-2.31.so /lib64/ld-linux-x86-64.so.2 \
    && mkdir -p /opt/etc \
    && addgroup -S -g 101 clickhouse \
    && adduser -S -h "${DEFAULT_DATA_DIR}" -s /bin/bash -G clickhouse -g "ClickHouse server" -u 101 clickhouse

RUN cd /tmp \
    && for package in ${PACKAGES}; do \
            echo "Get ${REPOSITORY}/${package}-${VERSION}-amd64.tgz" \
            && wget -c -q "${REPOSITORY}/${package}-${VERSION}-amd64.tgz" \
            && wget -c -q "${REPOSITORY}/${package}-${VERSION}-amd64.tgz.sha512" \
        ; done \
    && cat *.tgz.sha512 | sed 's:/output/:/tmp/:' | sha512sum -c \
    && for file in *.tgz; do \
        if [ -f "$file" ]; then \
            echo "Unpacking $file"; \
            tar xvzf "$file" --strip-components=1 -C /; \
        fi \
    ; done \
    && rm /tmp/*.tgz /install -r \
    && apk add --no-cache tzdata xmlstarlet \
    && cp /usr/share/zoneinfo/UTC /etc/localtime \
    && echo "UTC" > /etc/timezone \
    && mv /etc/clickhouse-server /opt/etc/ && mv /etc/clickhouse-client /opt/etc/ \
    && mkdir /opt/etc/clickhouse-server/config.d

COPY rootfs /
