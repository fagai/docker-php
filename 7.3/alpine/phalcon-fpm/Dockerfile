FROM php:7.3-fpm-alpine3.10

RUN apk add --no-cache ca-certificates && \
  update-ca-certificates

RUN apk add --no-cache git bash curl curl-dev oniguruma-dev icu-dev libpng-dev libxml2-dev libzip-dev openssl openssl-dev

RUN docker-php-ext-install pdo pdo_mysql curl mbstring bcmath opcache intl gd ctype json xml zip

RUN apk add --no-cache autoconf gcc g++ make imagemagick-dev && \
    pecl install imagick && \
    docker-php-ext-enable imagick && \
    apk add --no-cache libmemcached-dev re2c zlib-dev && \
    pecl install memcached && \
     docker-php-ext-enable memcached && \
    apk del --purge autoconf gcc g++ make && \
    pecl clear-cache

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# fast composer
RUN composer global require hirak/prestissimo && \
    composer clear-cache

ENV PHALCON_VERSION 3.4.4

# install phalcon
RUN curl -LO https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz \
    && tar xzf v${PHALCON_VERSION}.tar.gz \
    && docker-php-ext-install ${PWD}/cphalcon-${PHALCON_VERSION}/build/php7/64bits \
    && rm -rf v${PHALCON_VERSION}.tar.gz cphalcon-${PHALCON_VERSION}

# delete build packages
RUN apk del --purge re2c
