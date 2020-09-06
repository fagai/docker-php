FROM php:7.2-fpm

# set environment
ENV COMPOSER_HOME=/root/composer \
    PATH=$COMPOSER_HOME/vendor/bin:$PATH \
    COMPOSER_ALLOW_SUPERUSER=1

# package install
RUN apt-get update \
    && apt-get install -y --no-install-recommends zlib1g-dev curl libcurl4 libcurl4-gnutls-dev unzip libzip-dev libicu-dev libxml2-dev libonig-dev \
    && docker-php-ext-install -j$(nproc) pdo pdo_mysql curl mbstring bcmath opcache intl ctype json xml zip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# install prestissimo
RUN composer global require hirak/prestissimo \
    && composer clear-cache