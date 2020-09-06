FROM fagai/php:7.2-fpm

# set environment
ENV COMPOSER_HOME=/root/composer \
    PATH=$COMPOSER_HOME/vendor/bin:$PATH \
    COMPOSER_ALLOW_SUPERUSER=1 \
    PHALCON_VERSION=3.4.5

ENV IMAGE_LIBRARY \
    libfreetype6-dev libjpeg-dev libpng-dev libwebp-dev libghc-bmp-dev

# install gd, imagick, exif, memcached
RUN export CFLAGS="$PHP_CFLAGS" CPPFLAGS="$PHP_CPPFLAGS" LDFLAGS="$PHP_LDFLAGS" \
    && apt-get update \
    && apt-get install -y --no-install-recommends libmagickwand-dev libz-dev libmemcached-dev ${IMAGE_LIBRARY} \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-webp-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd exif \
    && pecl install imagick memcached \
    && docker-php-ext-enable imagick memcached \
    && pecl clear-cache \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# install phalcon
RUN curl -LO https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz \
    && tar xzf v${PHALCON_VERSION}.tar.gz \
    && docker-php-ext-install -j$(nproc) ${PWD}/cphalcon-${PHALCON_VERSION}/build/php7/64bits \
    && rm -rf v${PHALCON_VERSION}.tar.gz cphalcon-${PHALCON_VERSION}
