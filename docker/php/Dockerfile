FROM php:8.0-fpm-alpine3.13

ARG ENABLE_XDEBUG

RUN apk update

RUN apk add --no-cache autoconf build-base

RUN docker-php-ext-configure opcache
RUN docker-php-ext-install -j "$(getconf _NPROCESSORS_ONLN)" opcache

RUN if [ "$ENABLE_XDEBUG" -eq 1 ] ; then \
        pecl install xdebug-3.0.4 ; \
        docker-php-ext-enable xdebug ; \
    fi

COPY --from=composer:2.0 /usr/bin/composer /usr/local/bin/composer

COPY docker /var/www/html/docker

COPY docker/php/docker-php-ext-opcache.ini /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini

RUN if [ "$ENABLE_XDEBUG" -eq 1 ] ; then \
        cp docker/php/docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini ; \
    fi

EXPOSE 9000

CMD ["php-fpm"]
