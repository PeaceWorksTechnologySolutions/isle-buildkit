# syntax=docker/dockerfile:1.2.1
ARG repository=local
ARG tag=latest
ARG alpine=3.16.2
FROM alpine:${alpine} AS cache
FROM --platform=$BUILDPLATFORM ${repository}/composer:${tag} AS composer
FROM ${repository}/base:${tag}

RUN echo -e "https://dl-cdn.alpinelinux.org/alpine/v3.16/main\nhttps://dl-cdn.alpinelinux.org/alpine/v3.16/community" >/etc/apk/repositories && \
    apk update

RUN --mount=type=cache,id=nginx-apk,sharing=locked,from=cache,target=/var/cache/apk \
    apk add \
        nginx \
        php8 \
        php8-ctype \
        php8-curl \
        php8-dom \
        php8-fileinfo \
        php8-fpm \
        php8-gd \
        php8-iconv \
        php8-intl \
        php8-json \
        php8-ldap \
        php8-mbstring \
        php8-mysqli \
        php8-opcache \
        php8-openssl \
        php8-pdo \
        php8-pdo_mysql \
        php8-pdo_pgsql \
        php8-phar \
        php8-session \
        php8-simplexml \
        php8-tokenizer \
        php8-xml \
        php8-xmlreader \
        php8-xmlwriter \
        php8-xsl \
        php8-zip \
    && \
    cleanup.sh

ENV \
    NGINX_CLIENT_BODY_TIMEOUT=60s \
    NGINX_CLIENT_MAX_BODY_SIZE=0 \
    NGINX_ERROR_LOG_LEVEL=warn \
    NGINX_FASTCGI_CONNECT_TIMEOUT=60s \
    NGINX_FASTCGI_READ_TIMEOUT=60s \
    NGINX_FASTCGI_SEND_TIMEOUT=60s \
    NGINX_KEEPALIVE_TIMEOUT=75s \
    NGINX_LINGERING_TIMEOUT=5s \
    NGINX_PROXY_CONNECT_TIMEOUT=60s \
    NGINX_PROXY_READ_TIMEOUT=60s \
    NGINX_PROXY_SEND_TIMEOUT=60s \
    NGINX_SEND_TIMEOUT=60s \
    NGINX_WORKER_CONNECTIONS=1024 \
    NGINX_WORKER_PROCESSES=auto \
    PHP_DEFAULT_SOCKET_TIMEOUT=60 \
    PHP_LOG_LEVEL=notice \
    PHP_LOG_LIMIT=16384 \
    PHP_MAX_EXECUTION_TIME=30 \
    PHP_MAX_FILE_UPLOADS=20 \
    PHP_MAX_INPUT_TIME=60 \
    PHP_MAX_INPUT_VARS=3000 \
    PHP_MEMORY_LIMIT=256M \
    PHP_POST_MAX_SIZE=128M \
    PHP_PROCESS_CONTROL_TIMEOUT=60 \
    PHP_REQUEST_TERMINATE_TIMEOUT=60 \
    PHP_UPLOAD_MAX_FILESIZE=128M

COPY --from=composer /usr/bin/composer /usr/bin/composer

COPY rootfs /
