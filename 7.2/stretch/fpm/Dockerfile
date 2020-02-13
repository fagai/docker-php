FROM php:7.2-fpm

# set environment
ENV COMPOSER_HOME=/root/composer \
    PATH=$COMPOSER_HOME/vendor/bin:$PATH \
    COMPOSER_ALLOW_SUPERUSER=1

# package install
RUN apt-get update \
    && apt-get install -y zlib1g-dev curl libcurl4 libcurl4-gnutls-dev unzip libzip-dev libicu-dev libfreetype6-dev libjpeg62-turbo-dev libpng-dev libxml2-dev \
    && apt-get clean
RUN docker-php-ext-install pdo pdo_mysql curl mbstring bcmath opcache intl gd ctype json xml zip

RUN apt-get install -y libmagickwand-dev && \
    pecl install imagick && \
    docker-php-ext-enable imagick && \
    pecl clear-cache && \
    apt-get clean

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# install prestissimo
RUN composer global require hirak/prestissimo \
    && composer clear-cache
