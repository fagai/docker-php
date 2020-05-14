FROM fagai/php:7.3-alpine-fpm

RUN apk add --no-cache autoconf gcc g++ make && \
    apk add --no-cache libmemcached-dev re2c zlib-dev && \
    pecl install memcached && \
     docker-php-ext-enable memcached && \
    apk del --purge autoconf gcc g++ make && \
    pecl clear-cache

ENV PHALCON_VERSION 3.4.5

# install phalcon
RUN curl -LO https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz \
    && tar xzf v${PHALCON_VERSION}.tar.gz \
    && docker-php-ext-install ${PWD}/cphalcon-${PHALCON_VERSION}/build/php7/64bits \
    && rm -rf v${PHALCON_VERSION}.tar.gz cphalcon-${PHALCON_VERSION} \
    && docker-php-source delete \
    && apk del --purge re2c
