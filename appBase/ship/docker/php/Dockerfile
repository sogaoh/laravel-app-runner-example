FROM php:8.0-apache

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

ARG PHP_MODE

RUN mv "$PHP_INI_DIR/php.ini-$PHP_MODE" "$PHP_INI_DIR/php.ini" && \
    sed -ri -e 's!((post_max_size|upload_max_filesize) = ).M!\110M!g' $PHP_INI_DIR/php.ini && \
    sed -ri -e 's!/var/www/html!/var/www/html/public!g' /etc/apache2/sites-available/*.conf && \
    sed -ri -e 's!/var/www/!/var/www/html/public!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf && \
    apt-get update -y && \
    apt-get install -y unzip && \
    a2enmod rewrite



COPY lara-sample /var/www/html

WORKDIR /var/www/html


ARG NO_DEV_OPT
RUN composer install ${NO_DEV_OPT}

RUN if [ -n "${NO_DEV_OPT}" ]; then \
        rm -rf tests; \
        rm -rf _ide_helper*; \
        rm -rf .php_cs*; \
        rm -rf phpunit*; \
    fi

#RUN php artisan cache:clear && \
#    php artisan config:clear && \
#    php artisan route:clear && \
#    php artisan view:clear

RUN chown -R www-data:www-data .
RUN chmod -R a+w storage && \
    chmod -R a+w bootstrap/cache
