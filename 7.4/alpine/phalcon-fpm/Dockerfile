FROM fagai/php:7.4-alpine-fpm

RUN apk add --no-cache libmemcached-dev autoconf g++ re2c zlib-dev make && \
    pecl install memcached && docker-php-ext-enable memcached

ENV PHALCON_VERSION 4.0.2

RUN composer global require phalcon/zephir

# install php-psr
RUN pecl install psr && docker-php-ext-enable psr

ENV ZEPHIR_PARSER_VERSION 1.3.3

# install zephir-parser
RUN curl -LO https://github.com/phalcon/php-zephir-parser/archive/v${ZEPHIR_PARSER_VERSION}.tar.gz \
    && tar xzf v${ZEPHIR_PARSER_VERSION}.tar.gz \
    && cd php-zephir-parser-${ZEPHIR_PARSER_VERSION} \
    && phpize \
    && ./configure \
    && make \
    && make install \
    && docker-php-ext-enable zephir_parser \
    && cd .. && rm -rf php-zephir-parser-${ZEPHIR_PARSER_VERSION} v${ZEPHIR_PARSER_VERSION}.tar.gz

# install phalcon
RUN curl -LO https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz \
    && tar xzf v${PHALCON_VERSION}.tar.gz \
    && cd cphalcon-${PHALCON_VERSION} \
    && curl -LO https://github.com/phalcon/zephir/releases/download/0.12.15/zephir.phar \
    && php zephir.phar fullclean && php zephir.phar build \
    && cd .. \
    && rm -rf v${PHALCON_VERSION}.tar.gz cphalcon-${PHALCON_VERSION}

# delete build packages
RUN apk del gcc make g++ re2c autoconf
