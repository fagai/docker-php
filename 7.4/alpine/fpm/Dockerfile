FROM php:7.4-fpm-alpine3.10

# set japan repository
# RUN sed -i 's/http\:\/\/dl-cdn.alpinelinux.org/http\:\/\/ftp.tsukuba.wide.ad.jp\/Linux/g' /etc/apk/repositories

RUN apk add --no-cache ca-certificates && \
  update-ca-certificates

RUN apk add --no-cache bash curl curl-dev oniguruma-dev

RUN docker-php-ext-install pdo pdo_mysql curl mbstring

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# fast composer
# RUN composer config -g repos.packagist composer https://packagist.jp
RUN composer global require hirak/prestissimo && \
    composer clear-cache
