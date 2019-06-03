FROM php:7.2-cli

RUN apt-get update && apt-get -y upgrade
RUN apt-get install -y git libpq-dev zlib1g-dev libxml2-dev
RUN apt-get install -y -t testing golang-1.11

RUN docker-php-ext-install intl
RUN docker-php-ext-install soap
RUN docker-php-ext-install pcntl
RUN docker-php-ext-install pdo_pgsql
RUN docker-php-ext-install zip
