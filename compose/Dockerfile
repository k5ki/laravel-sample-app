FROM php:8.1.4-apache-buster

COPY php/php.ini /usr/local/etc/php/php.ini
RUN docker-php-ext-configure opcache --enable-opcache && \
    docker-php-ext-install pdo pdo_mysql
COPY php/conf.d/opcache.ini /usr/local/etc/php/conf.d/opcache.ini

COPY apache/000-default.conf /etc/apache2/sites-available/000-default.conf

RUN a2enmod rewrite

# enable xdebug
# see https://www.jetbrains.com/help/phpstorm/configuring-xdebug.html#configuring-xdebug-docker
RUN pecl install xdebug-3.1.4 && \
    docker-php-ext-enable xdebug && \
    echo "xdebug.mode=debug" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.client_host = host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# install dev tools
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
RUN apt update && apt install -y \
    dnsutils \
    git \
    less \
    lsof \
    net-tools \
    vim \
    wget \
 && apt clean
