FROM php:8.1-fpm-alpine

RUN apk add --no-cache ca-certificates && \
    update-ca-certificates

RUN apk add --no-cache bash curl curl-dev oniguruma-dev icu-dev libxml2-dev libzip-dev openssl openssl-dev  

RUN php -m

# installed: pdo, pdo_sqlite, openssl, ctype, curl, mbstring, xml
RUN docker-php-ext-install -j$(nproc) pdo_mysql bcmath opcache intl zip

RUN apk add --no-cache postgresql-client postgresql-dev && \
    docker-php-ext-install -j$(nproc) pdo_pgsql && \
    apk del postgresql-dev

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && composer clear-cache
