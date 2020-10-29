FROM php:7.2-fpm-alpine

RUN apk add --no-cache ca-certificates && \
    update-ca-certificates

RUN apk add --no-cache bash curl curl-dev oniguruma-dev icu-dev libxml2-dev libzip-dev openssl openssl-dev

RUN docker-php-ext-install pdo pdo_mysql curl mbstring bcmath opcache intl ctype json xml zip

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && composer clear-cache

