ARG BUILD_FROM
FROM $BUILD_FROM

ARG RABBITMQ_VERSION="3.13.7-r0"
ARG RABBITMQ_HOME="/data/rabbitmq"
ARG RABBITMQ_CONFIG="/data/config"

ENV LANG=C.UTF-8 \
    LANGUAGE=C.UTF-8 \
    LC_ALL=C.UTF-8

RUN apk add --no-cache rabbitmq-server=${RABBITMQ_VERSION} procps tzdata

RUN rm -rf /var/lib/rabbitmq /etc/rabbitmq && \
    ln -sf "${RABBITMQ_HOME}" /var/lib/rabbitmq && \
    ln -sf "${RABBITMQ_CONFIG}" /etc/rabbitmq && \
    mkdir -p "${RABBITMQ_CONFIG}" "${RABBITMQ_CONFIG}/conf.d" /tmp/rabbitmq-ssl /var/log/rabbitmq && \
	chown -fR rabbitmq:rabbitmq "${RABBITMQ_CONFIG}" /tmp/rabbitmq-ssl /var/log/rabbitmq && \
	chmod 1777 "${RABBITMQ_CONFIG}" "${RABBITMQ_CONFIG}/conf.d" /tmp/rabbitmq-ssl /var/log/rabbitmq

EXPOSE 1883 5672 15672 15692 25672

COPY rootfs /
