FROM php:7.2-fpm-alpine3.7

# set japan repository
RUN sed -i 's/http\:\/\/dl-cdn.alpinelinux.org/http\:\/\/ftp.tsukuba.wide.ad.jp\/Linux/g' /etc/apk/repositories

RUN apk upgrade && \
  apk update && \
  apk add ca-certificates && \
  update-ca-certificates

RUN apk add --update bash curl curl-dev

RUN docker-php-ext-install pdo pdo_mysql curl mbstring

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# fast composer
RUN composer config -g repos.packagist composer https://packagist.jp
RUN composer global require hirak/prestissimo

