FROM php:7.3-fpm

# set environment
ENV COMPOSER_HOME=/root/composer \
    PATH=$COMPOSER_HOME/vendor/bin:$PATH \
    COMPOSER_ALLOW_SUPERUSER=1

# set package repository
RUN curl -sL http://www.debian.or.jp/using/apt/sources.list.http.ftp.jp.debian.org > /etc/apt/sources.list && \
    apt-get update

# package install
RUN apt-get install -y zlib1g-dev curl libcurl4 libcurl4-gnutls-dev libzip-dev
RUN docker-php-ext-install pdo pdo_mysql zip curl mbstring

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    chmod +x /usr/local/bin/composer && \
    composer --ansi --version

# set packagist.jp
RUN composer config -g repos.packagist composer https://packagist.jp

# install prestissimo
RUN mkdir -p $COMPOSER_HOME && \
    composer global require hirak/prestissimo
