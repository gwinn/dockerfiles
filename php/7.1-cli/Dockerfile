FROM php:7.1-cli

RUN apt-get update && apt-get -y upgrade && apt-get install -y git \
 libpq-dev \
 zlib1g-dev \
 libxml2-dev

RUN docker-php-ext-install intl
RUN docker-php-ext-install soap
RUN docker-php-ext-install pcntl
RUN docker-php-ext-install pdo_pgsql
RUN docker-php-ext-install zip

RUN pecl install xdebug-2.6.0
RUN pecl install redis-4.0.1
RUN docker-php-ext-enable redis xdebug
