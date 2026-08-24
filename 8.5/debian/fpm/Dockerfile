FROM php:8.5-fpm

# set environment
ENV COMPOSER_HOME=/root/composer

ENV PATH=$COMPOSER_HOME/vendor/bin:$PATH \
    COMPOSER_ALLOW_SUPERUSER=1

RUN php -m

# package install
RUN apt-get update \
    && apt-get install -y --no-install-recommends zlib1g-dev curl libcurl4 libcurl4-gnutls-dev unzip libzip-dev libicu-dev libxml2-dev libonig-dev libpq-dev  \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# installed: pdo, pdo_sqlite, openssl, ctype, curl, mbstring, xml
RUN docker-php-ext-install -j$(nproc) pdo_mysql pdo_pgsql bcmath intl zip

# Install Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer
