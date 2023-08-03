FROM php:8.2-fpm

#php.ini
COPY ./conf/php/ini/dev/php.ini /usr/local/etc/php/php.ini

# Install system dependencies and PHP extensions
RUN apt-get update && apt-get install -y \
    libzip-dev \
    libfreetype-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libicu-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-configure intl \
    && docker-php-ext-install -j$(nproc) gd 

RUN docker-php-ext-install mysqli pdo pdo_mysql zip exif intl opcache