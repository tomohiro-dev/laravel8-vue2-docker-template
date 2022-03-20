FROM php:8.0-fpm-buster
LABEL maintainer="soregashi-27"
SHELL ["bin/bash", "-oeux", "pipefail", "-c"]

# タイムゾーン環境
ENV TZ=UTC \
  LANG=en_US.UTF-8 \
  LANGUAGE=en_US:en \
  LC_ALL=en_US.UTF-8 \
  # Composer
  COMPOSER_ALLOW_SUPERUSER=1 \
  COMPOSER_HOME=/composer

COPY --from=composer:2.0 /usr/bin/composer /usr/bim/composer

RUN apt-get update && \
    apt-get -y install git libicu-dev libonig-dev libzip-dev unzip locales && \
    apt-get celan && \
    rm -rf /var/lib/apt/lists/* && \
    locale-gen en_US.UTF-8 && \
    docker-php-ext-install intl pdo_mysql zip bcmath && \
    composer config -g process-timeout 3600 && \
    composer config -g repos.packagist composer https://packagist.org
    
COPY ./infra/docker/php-fpm.d/zzz-www.conf /usr/local/etc/php-fpm.d/zzz-www.conf
COPY ./infra/docker/php/php.ini /usr/local/etc/php/php.ini