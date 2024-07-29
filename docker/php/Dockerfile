# Base stage
FROM dunglas/frankenphp AS base

RUN install-php-extensions \
    bcmath \
    pcntl \
    exif \
    gd \
    zip \
    pdo_pgsql \
    redis \
    intl

# Create a non-root user
RUN adduser --disabled-password --gecos '' appuser

# Set working directory and copy application files
WORKDIR /app
COPY --chown=appuser:appuser . .

# Install Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Switch to non-root user
USER appuser

# Development stage
FROM base AS development

RUN composer install

ENTRYPOINT ["php", "artisan", "octane:frankenphp", "--max-requests=1"]

# Production stage
FROM base AS production

RUN composer install --no-dev --optimize-autoloader

ENTRYPOINT ["php", "artisan", "octane:frankenphp"]
