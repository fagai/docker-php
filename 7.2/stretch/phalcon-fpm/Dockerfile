FROM php:7.2-fpm

# set environment
ENV COMPOSER_HOME=/root/composer \
    PATH=$COMPOSER_HOME/vendor/bin:$PATH \
    COMPOSER_ALLOW_SUPERUSER=1 \
    PHALCON_VERSION=3.2.4

# set package repository
RUN curl -sL http://www.debian.or.jp/using/apt/sources.list.http.ftp.jp.debian.org > /etc/apt/sources.list && \
    apt-get update

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
    composer global require hirak/prestissimio

# install phalcon
RUN curl -sSL "https://codeload.github.com/phalcon/cphalcon/tar.gz/v${PHALCON_VERSION}" | tar -xz \
    && cd cphalcon-${PHALCON_VERSION}/build \
    && ./install \
    && cp ../tests/_ci/phalcon.ini $(php-config --configure-options | grep -o "with-config-file-scan-dir=\([^ ]*\)" | awk -F'=' '{print $2}') \
    && cd ../../ \
    && rm -r cphalcon-${PHALCON_VERSION}

# install phalcon devtools
RUN composer global require phalcon/devtools \
    && ln -s $COMPOSER_HOME/vendor/phalcon/devtools/phalcon.php /usr/local/bin/phalcon \
    && chmod ugo+x /usr/local/bin/phalcon
