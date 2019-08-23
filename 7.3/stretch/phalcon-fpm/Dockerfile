FROM php:7.3-fpm

# set environment
ENV COMPOSER_HOME=/root/composer \
    PATH=$COMPOSER_HOME/vendor/bin:$PATH \
    COMPOSER_ALLOW_SUPERUSER=1 \
    PHALCON_VERSION=3.4.4

# set package repository
RUN apt-get update

# package install
RUN buildDeps="libxml2-dev zlib1g-dev" && \ 
    apt-get install -y ${buildDeps} curl libcurl4-openssl-dev unzip git libzip-dev && \
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

# install phalcon
RUN curl -LO https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz \
    && tar xzf v${PHALCON_VERSION}.tar.gz \
    && docker-php-ext-install ${PWD}/cphalcon-${PHALCON_VERSION}/build/php7/64bits \
    && rm -rf v${PHALCON_VERSION}.tar.gz cphalcon-${PHALCON_VERSION}

# install phalcon devtools
#RUN composer global require phalcon/devtools \
#    && ln -s $COMPOSER_HOME/vendor/phalcon/devtools/phalcon.php /usr/local/bin/phalcon \
#    && chmod ugo+x /usr/local/bin/phalcon
