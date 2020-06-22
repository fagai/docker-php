FROM php:7.3-fpm

# set environment
ENV COMPOSER_HOME=/root/composer \
    PATH=$COMPOSER_HOME/vendor/bin:$PATH \
    COMPOSER_ALLOW_SUPERUSER=1

ENV IMAGE_LIBRARY \
    libfreetype6-dev libjpeg-dev libpng-dev libwebp-dev libghc-bmp-dev

# package install
RUN apt-get update \
    && apt-get install -y --no-install-recommends zlib1g-dev curl libcurl4 libcurl4-gnutls-dev unzip libzip-dev libicu-dev libxml2-dev ${IMAGE_LIBRARY} libonig-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-webp-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) pdo pdo_mysql curl mbstring bcmath opcache intl gd ctype json xml zip exif \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get update \
    && apt-get install -y --no-install-recommends libmagickwand-dev \
    && pecl install imagick \
    && docker-php-ext-enable imagick \
    && pecl clear-cache \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# install prestissimo
RUN composer global require hirak/prestissimo \
    && composer clear-cache
