FROM php:7.3.8-fpm-alpine as builder

MAINTAINER inkiddream <inkiddream@gmail.com>

# Установка пакетов, необходимых для сборки
RUN apk add --no-cache postgresql-dev autoconf git unzip openssh $PHPIZE_DEPS libxml2-dev gcc libpng-dev libc-dev imap-dev libzip-dev

RUN apk add curl
RUN apk add zip

# Установка необходимых экстеншенов
RUN yes "" | pecl install xdebug && docker-php-ext-enable xdebug
RUN docker-php-ext-install gd
RUN docker-php-ext-configure imap --with-imap --with-imap-ssl \
    && docker-php-ext-install imap
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install pdo_pgsql
RUN pecl install redis && docker-php-ext-enable redis
RUN docker-php-ext-install xml
RUN docker-php-ext-install zip
RUN docker-php-ext-install mysqli pdo && docker-php-ext-enable pdo_mysql
RUN docker-php-ext-install mbstring
RUN docker-php-ext-install soap

# Установка композера
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && ln -s $(composer config --global home) /root/composer
ENV PATH=$PATH:/root/composer/vendor/bin COMPOSER_ALLOW_SUPERUSER=1