FROM php:8.3-fpm

ARG BUILD_ENV=production

# Instalar dependencias del sistema
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    zip \
    unzip \
    supervisor \
    && docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd zip opcache \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Instalar Redis extension
RUN pecl install redis && docker-php-ext-enable redis

# Instalar Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Configurar directorio de trabajo
WORKDIR /var/www

# Copiar código fuente
COPY ./src /var/www

# Crear directorios si no existen y configurar permisos
RUN mkdir -p /var/www/storage/framework/views \
    /var/www/storage/framework/cache \
    /var/www/storage/framework/sessions \
    /var/www/storage/logs \
    /var/www/bootstrap/cache \
    && chown -R www-data:www-data /var/www/storage /var/www/bootstrap/cache \
    && chmod -R 775 /var/www/storage /var/www/bootstrap/cache

# Instalar dependencias de Composer sin paquetes de desarrollo
RUN composer install --no-dev --optimize-autoloader --no-interaction --prefer-dist

# Configurar supervisor
RUN mkdir -p /var/log/supervisor

# Optimizaciones de Laravel
RUN php artisan config:cache && \
    php artisan route:cache && \
    php artisan view:cache && \
    php artisan event:cache

EXPOSE 9000

CMD ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
