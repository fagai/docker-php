FROM php:7.3-fpm-alpine3.10

# set japan repository
RUN sed -i 's/http\:\/\/dl-cdn.alpinelinux.org/http\:\/\/ftp.tsukuba.wide.ad.jp\/Linux/g' /etc/apk/repositories

RUN apk upgrade && \
  apk update && \
  apk add ca-certificates && \
  update-ca-certificates

RUN apk add --update bash curl curl-dev gcc make git unzip

RUN docker-php-ext-install pdo pdo_mysql curl mbstring

RUN apk add --update --no-cache libmemcached-dev autoconf g++ re2c zlib-dev && \
    pecl install memcached && docker-php-ext-enable memcached

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# fast composer
RUN composer config -g repos.packagist composer https://packagist.jp
RUN composer global require hirak/prestissimo

ENV PHALCON_VERSION 3.4.4

# install phalcon
RUN curl -LO https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz \
    && tar xzf v${PHALCON_VERSION}.tar.gz \
    && docker-php-ext-install ${PWD}/cphalcon-${PHALCON_VERSION}/build/php7/64bits \
    && rm -rf v${PHALCON_VERSION}.tar.gz cphalcon-${PHALCON_VERSION}

# delete build packages
RUN apk del gcc make g++ re2c autoconf

# install phalcon devtools
#RUN composer global require phalcon/devtools \
#    && ln -s /root/.composer/vendor/phalcon/devtools/phalcon.php /usr/local/bin/phalcon \
#    && chmod ugo+x /usr/local/bin/phalcon
