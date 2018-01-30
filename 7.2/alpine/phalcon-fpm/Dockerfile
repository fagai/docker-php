FROM php:7.2-fpm-alpine3.7

# set japan repository
RUN sed -i 's/http\:\/\/dl-cdn.alpinelinux.org/http\:\/\/ftp.tsukuba.wide.ad.jp\/Linux/g' /etc/apk/repositories

RUN apk upgrade && \
  apk update && \
  apk add ca-certificates && \
  update-ca-certificates

RUN apk add --update bash curl curl-dev gcc make

RUN docker-php-ext-install pdo pdo_mysql curl mbstring

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# fast composer
RUN composer config -g repos.packagist composer https://packagist.jp
RUN composer global require hirak/prestissimo

ENV PHALCON_VERSION 3.3.1

RUN apk add --update autoconf g++ re2c

# install phalcon
RUN curl -sSL "https://codeload.github.com/phalcon/cphalcon/tar.gz/v${PHALCON_VERSION}" | tar -xz \
    && cd cphalcon-${PHALCON_VERSION}/build \
    && ./install \
    && cp ../tests/_ci/phalcon.ini $(php-config --configure-options | grep -o "with-config-file-scan-dir=\([^ ]*\)" | awk -F'=' '{print $2}') \
    && cd ../../ \
    && rm -r cphalcon-${PHALCON_VERSION}

# install phalcon devtools
RUN composer global require phalcon/devtools \
    && ln -s /root/.composer/vendor/phalcon/devtools/phalcon.php /usr/local/bin/phalcon \
    && chmod ugo+x /usr/local/bin/phalcon
