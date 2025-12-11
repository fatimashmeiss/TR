# Stage 1: PHP + composer build
FROM php:8.2-fpm AS builder

# Install system deps
RUN apt-get update && apt-get install -y \
    zip unzip git curl libpng-dev libonig-dev libxml2-dev && \
    docker-php-ext-install pdo pdo_mysql

# Copy backend source code
WORKDIR /var/www/html
COPY . .

# Install composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Install PHP dependencies
RUN composer install --no-dev --optimize-autoloader

# Stage 2: production image
FROM php:8.2-fpm

WORKDIR /var/www/html

# Install PHP extensions
RUN apt-get update && apt-get install -y \
    zip unzip libpng-dev libonig-dev libxml2-dev && \
    docker-php-ext-install pdo pdo_mysql

COPY --from=builder /var/www/html .

# Expose port 80
EXPOSE 80

CMD ["php", "-S", "0.0.0.0:80", "-t", "public"]
