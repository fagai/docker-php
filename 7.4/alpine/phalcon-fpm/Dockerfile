FROM php:7.4-fpm-alpine3.11

# set japan repository
RUN sed -i 's/http\:\/\/dl-cdn.alpinelinux.org/http\:\/\/ftp.tsukuba.wide.ad.jp\/Linux/g' /etc/apk/repositories

RUN apk upgrade && \
  apk update && \
  apk add ca-certificates && \
  update-ca-certificates

RUN apk add --update bash curl curl-dev oniguruma-dev gcc make git unzip

RUN docker-php-ext-install pdo pdo_mysql curl mbstring

RUN apk add --update --no-cache libmemcached-dev autoconf g++ re2c zlib-dev && \
    pecl install memcached && docker-php-ext-enable memcached

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# fast composer
RUN composer config -g repos.packagist composer https://packagist.jp
RUN composer global require hirak/prestissimo

ENV PHALCON_VERSION 4.0.0-rc.3

RUN composer global require phalcon/zephir

# install php-psr
RUN pecl install psr && docker-php-ext-enable psr

# install zephir-parser
RUN curl -LO https://github.com/phalcon/php-zephir-parser/archive/v1.3.2.tar.gz \
    && tar xzf v1.3.2.tar.gz \
    && cd php-zephir-parser-1.3.2 \
    && phpize \
    && ./configure \
    && make \
    && make install \
    && docker-php-ext-enable zephir_parser \
    && cd .. && rm -rf php-zephir-parser-1.3.2 v1.3.2.tar.gz

# install phalcon
RUN curl -LO https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz \
    && tar xzf v${PHALCON_VERSION}.tar.gz \
    && cd cphalcon-${PHALCON_VERSION} \
    && curl -LO https://github.com/phalcon/zephir/releases/download/0.12.12/zephir.phar \
    && php zephir.phar fullclean && php zephir.phar build \
    && cd .. \
    && rm -rf v${PHALCON_VERSION}.tar.gz cphalcon-${PHALCON_VERSION}

# delete build packages
RUN apk del gcc make g++ re2c autoconf
