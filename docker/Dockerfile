FROM composer:2.3.3 as build
COPY laravel-sample-app/ /app
RUN composer install --prefer-dist --no-dev --optimize-autoloader --no-interaction

FROM php:8.1.4-apache-buster as production

COPY docker/php/php.ini /usr/local/etc/php/php.ini
RUN docker-php-ext-configure opcache --enable-opcache && \
    docker-php-ext-install pdo pdo_mysql
COPY docker/php/conf.d/opcache.ini /usr/local/etc/php/conf.d/opcache.ini

COPY --from=build /app /var/www/html
COPY docker/apache/000-default.conf /etc/apache2/sites-available/000-default.conf

RUN php artisan config:cache && \
    php artisan route:cache && \
    chmod 777 -R /var/www/html/storage/ && \
    chown -R www-data:www-data /var/www/ && \
    a2enmod rewrite
