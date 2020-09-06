FROM fagai/php:7.2-alpine-fpm

ENV PHALCON_VERSION 3.4.5
ENV IMAGE_LIBRARY \
    libjpeg-turbo-dev freetype-dev libpng-dev 

RUN apk add --no-cache autoconf gcc g++ make imagemagick-dev ${IMAGE_LIBRARY} \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd exif \
    && pecl install imagick \
    && docker-php-ext-enable imagick \
    && apk del --purge autoconf gcc g++ make

RUN apk add --no-cache autoconf gcc g++ make && \
    apk add --no-cache libmemcached-dev re2c zlib-dev && \
    pecl install memcached && \
    docker-php-ext-enable memcached && \
    apk del --purge autoconf gcc g++ make && \
    pecl clear-cache

# install phalcon
RUN curl -LO "https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz" \
    && tar xzf v${PHALCON_VERSION}.tar.gz \
    && docker-php-ext-install "${PWD}/cphalcon-${PHALCON_VERSION}/build/php7/64bits" \
    && rm -rf v${PHALCON_VERSION}.tar.gz cphalcon-${PHALCON_VERSION} \
    && docker-php-source delete \
    && apk del --purge re2c
