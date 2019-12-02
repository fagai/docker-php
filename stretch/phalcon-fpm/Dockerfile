FROM php:7.4-fpm

# set environment
ENV COMPOSER_HOME=/root/composer \
    PATH=$COMPOSER_HOME/vendor/bin:$PATH \
    COMPOSER_ALLOW_SUPERUSER=1 \
    PHALCON_VERSION=4.0.0-rc.3

# set package repository
RUN apt-get update

# package install
RUN buildDeps="libxml2-dev" && \ 
    apt-get install -y ${buildDeps} curl libcurl4-openssl-dev unzip git libzip-dev libonig-dev && \
    docker-php-ext-install pdo pdo_mysql zip curl mbstring && \
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false $buildDeps && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    chmod +x /usr/local/bin/composer && \
    composer --ansi --version

# set packagist.jp
RUN composer config -g repos.packagist composer https://packagist.jp

# install prestissimo
RUN mkdir -p $COMPOSER_HOME && \
    composer global require hirak/prestissimo

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
