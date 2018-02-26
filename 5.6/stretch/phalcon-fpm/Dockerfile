FROM php:5.6-fpm

# set environment
ENV COMPOSER_HOME=/root/composer \
    PATH=$COMPOSER_HOME/vendor/bin:$PATH \
    COMPOSER_ALLOW_SUPERUSER=1 \
    PHALCON_VERSION=3.2.4

RUN apt-get update

# package install
RUN apt-get install -y zlib1g-dev curl libcurl3 libcurl4-gnutls-dev
RUN docker-php-ext-install pdo pdo_mysql zip curl mbstring

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
RUN curl -sSL "https://codeload.github.com/phalcon/cphalcon/tar.gz/v${PHALCON_VERSION}" | tar -xz \
    && cd cphalcon-${PHALCON_VERSION}/build \
    && ./install \
    && cp ../tests/_ci/phalcon.ini $(php-config --configure-options | grep -o "with-config-file-scan-dir=\([^ ]*\)" | awk -F'=' '{print $2}') \
    && cd ../../ \
    && rm -r cphalcon-${PHALCON_VERSION}

