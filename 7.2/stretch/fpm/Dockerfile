FROM php:7.2-fpm

ENV COMPOSER_HOME=/root/composer \
    PATH=$COMPOSER_HOME/vendor/bin:$PATH \
    COMPOSER_ALLOW_SUPERUSER=1

# パッケージ取得先を日本に変更
RUN curl -sL http://www.debian.or.jp/using/apt/sources.list.http.ftp.jp.debian.org > /etc/apt/sources.list \
  && apt-get update

# package install
RUN apt-get install -y zlib1g-dev curl libcurl3 libcurl4-gnutls-dev
RUN docker-php-ext-install pdo pdo_mysql zip curl mbstring

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    chmod +x /usr/local/bin/composer && \
    composer --ansi --version

# packagist.jpを見る
RUN composer config -g repos.packagist composer https://packagist.jp
