# Stage 1: build Laravel app with Composer
FROM php:8.2-fpm AS builder

# Install system dependencies + PHP extensions
RUN apt-get update && apt-get install -y \
    zip unzip git curl libpng-dev libonig-dev libxml2-dev && \
    docker-php-ext-install pdo pdo_mysql


WORKDIR /var/www/html


COPY backend/ .


COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN composer install --no-dev --optimize-autoloader

FROM php:8.2-fpm

WORKDIR /var/www/html


RUN apt-get update && apt-get install -y \
    zip unzip libpng-dev libonig-dev libxml2-dev && \
    docker-php-ext-install pdo pdo_mysql

COPY --from=builder /var/www/html .


EXPOSE 80


CMD ["php", "-S", "0.0.0.0:80", "-t", "public"]

CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=80"]

