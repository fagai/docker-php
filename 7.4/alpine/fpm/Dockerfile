FROM php:7.4-fpm-alpine

RUN apk add --no-cache ca-certificates && \
  update-ca-certificates

RUN apk add --no-cache bash curl curl-dev oniguruma-dev icu-dev libpng-dev libxml2-dev libzip-dev openssl openssl-dev

RUN docker-php-ext-install pdo pdo_mysql curl mbstring bcmath opcache intl gd ctype json xml zip exif

RUN apk add --no-cache autoconf gcc g++ make imagemagick-dev && \
    pecl install imagick && \
    docker-php-ext-enable imagick && \
    apk del --purge autoconf gcc g++ make && \
    pecl clear-cache

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# fast composer
# RUN composer config -g repos.packagist composer https://packagist.jp
RUN composer global require hirak/prestissimo && \
    composer clear-cache
