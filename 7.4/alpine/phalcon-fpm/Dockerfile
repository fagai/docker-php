FROM fagai/php:7.4-alpine-fpm

RUN apk add --no-cache autoconf gcc g++ make pcre-dev \
    && apk add --no-cache libmemcached-dev re2c zlib-dev \
    && pecl install memcached \
    && docker-php-ext-enable memcached \
    && pecl install psr \
    && docker-php-ext-enable psr \
    && apk del --purge autoconf gcc g++ make \
    && pecl clear-cache

ENV PHALCON_VERSION 4.0.6
ENV ZEPHIR_PARSER_VERSION 1.3.3
ENV ZEPHIR_VERSION 0.12.17

# install zephir-parser
RUN curl -LO https://github.com/phalcon/php-zephir-parser/archive/v${ZEPHIR_PARSER_VERSION}.tar.gz \
    && tar xzf v${ZEPHIR_PARSER_VERSION}.tar.gz \
    && docker-php-ext-install ${PWD}/php-zephir-parser-${ZEPHIR_PARSER_VERSION} \
    && rm -rf php-zephir-parser-${ZEPHIR_PARSER_VERSION} v${ZEPHIR_PARSER_VERSION}.tar.gz

# install phalcon extension
RUN apk add --no-cache autoconf gcc g++ make file \
    && curl -LO https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz \
    && tar xzf v${PHALCON_VERSION}.tar.gz \
    && cd cphalcon-${PHALCON_VERSION} \
    && curl -LO https://github.com/phalcon/zephir/releases/download/${ZEPHIR_VERSION}/zephir.phar \
    && php zephir.phar fullclean && php zephir.phar compile \
    && docker-php-ext-install ${PWD}/ext \
    && cd .. \
    && rm -rf v${PHALCON_VERSION}.tar.gz cphalcon-${PHALCON_VERSION} \
    && docker-php-source delete \
    && apk del gcc make g++ re2c autoconf file