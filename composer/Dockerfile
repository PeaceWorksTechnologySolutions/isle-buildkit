# syntax=docker/dockerfile:1.2.1
ARG repository=local
ARG tag=latest
ARG alpine=3.16.2
FROM --platform=$BUILDPLATFORM ${repository}/download:${tag} AS download 

# https://getcomposer.org/download/
RUN --mount=type=cache,id=download-downloads,sharing=locked,target=/opt/downloads \
    COMPOSER_VERSION="2.4.2" && \
    COMPOSER_FILE="composer.phar" && \
    COMPOSER_URL="https://getcomposer.org/download/${COMPOSER_VERSION}/${COMPOSER_FILE}" && \
    COMPOSER_SHA256="8fe98a01050c92cc6812b8ead3bd5b6e0bcdc575ce7a93b242bde497a31d7732" && \
    download.sh --url "${COMPOSER_URL}" --sha256 "${COMPOSER_SHA256}" "${DOWNLOAD_CACHE_DIRECTORY}" && \
    cp "${DOWNLOAD_CACHE_DIRECTORY}/${COMPOSER_FILE}" /usr/bin/composer && \
    chmod a+x /usr/bin/composer

FROM alpine:${alpine} AS cache
FROM ${repository}/download:${tag}

RUN echo -e "https://dl-cdn.alpinelinux.org/alpine/v3.16/main\nhttps://dl-cdn.alpinelinux.org/alpine/v3.16/community" >/etc/apk/repositories && \
    apk update

# Install packages and tools that allow for basic downloads.
RUN --mount=type=cache,id=composer-apk,sharing=locked,from=cache,target=/var/cache/apk \
    apk add --no-cache \
        php8 \
        php8-ctype \
        php8-curl \
        php8-dom \
        php8-fileinfo \
        php8-fpm \
        php8-gd \
        php8-iconv \
        php8-json \
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
        php8-xmlwriter \
        php8-xmlreader \
        php8-xsl \
    && \
    echo '' > /root/.ash_history

COPY --from=download /usr/bin/composer /usr/bin/composer

COPY rootfs /
