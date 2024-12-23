ARG PHP_VERSION=8.3
ARG COMPOSER_VERSION=2.6.5

FROM composer:${COMPOSER_VERSION} AS composer
FROM php:${PHP_VERSION}-fpm AS app_php

RUN apt update && apt install -y \
        zlib1g-dev \
        g++ git libicu-dev zip libzip-dev \
    	libfreetype6-dev \
    	libjpeg62-turbo-dev \
    	libpng-dev \
    && docker-php-ext-configure zip \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && pecl install apcu \
    && docker-php-ext-enable apcu \
    && docker-php-ext-install -j$(nproc) \
        intl \
        opcache \
        pdo \
        pdo_mysql \
        zip \
	    gd \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug

RUN rm -rf /etc/localtime && ln -s /usr/share/zoneinfo/Europe/Brussels /etc/localtime

RUN mkdir /usr/local/etc/php/conf.d/disabled
RUN rm /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# Copy the PHP-FPM configuration file
COPY docker/php/www.conf /usr/local/etc/php-fpm.d/www.conf

COPY --from=composer /usr/bin/composer /usr/bin/composer
COPY docker/php/php.ini /usr/local/etc/php/php.ini
COPY docker/php/php-cli.ini /usr/local/etc/php/php-cli.ini
COPY docker/php/conf.d/error_reporting.ini /usr/local/etc/php/conf.d/error_reporting.ini
COPY docker/php/conf.d/docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/disabled/docker-php-ext-xdebug.ini

# https://getcomposer.org/doc/03-cli.md#composer-allow-superuser
ENV COMPOSER_ALLOW_SUPERUSER=1
ENV COMPOSER_MEMORY_LIMIT=-1
ENV PATH="${PATH}:/root/.composer/vendor/bin"

WORKDIR /srv/app
