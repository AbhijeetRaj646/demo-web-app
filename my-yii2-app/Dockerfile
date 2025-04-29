# Use PHP image with required extensions
FROM php:8.1-fpm

# Install system dependencies and PHP extensions
RUN apt-get update && apt-get install -y \
    libzip-dev zip unzip \
    && docker-php-ext-install pdo pdo_mysql zip

# Install Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Set working directory
WORKDIR /var/www/html

# Copy app files
COPY . .

# Install PHP dependencies
RUN composer install

# Set permissions
RUN chmod -R 777 runtime web/assets

# Expose port 9000 (php-fpm default)
EXPOSE 9000

# Start PHP-FPM
CMD ["php-fpm"]
