ARG PHP_VERSION=8.4

FROM php:${PHP_VERSION}-fpm

# Set timezone
ENV TZ=UTC
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Install system packages (Added netcat-openbsd for DB check)
RUN apt-get update && apt-get install -y supervisor cron tzdata netcat-openbsd

# Install PHP extensions
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/
RUN install-php-extensions \
    @composer \
    apcu \
    gd \
    intl \
    memcached \
    mysqli \
    opcache \
    pdo_mysql \
    pdo_pgsql \
    redis \
    zip

# Configure www-data user home directory
RUN usermod -d /home/www-data -m www-data
RUN mkdir -p /home/www-data && chown www-data:www-data /home/www-data

# Copy OpenCart's original php.ini
COPY upload/php.ini /usr/local/etc/php/conf.d/opencart.ini

# --- COPY APPLICATION CODE ---
# 1. Copy source code to /var/www/upload (Initial content for the volume)
COPY upload/ /var/www/upload/

# 2. Copy Configs to system paths (NOT inside /var/www)
COPY ./docker/php/supervisor/supervisord.conf /etc/supervisor/supervisord.conf
COPY ./docker/php/supervisor/services/ /etc/supervisor/conf.d/services/

# 3. Copy Cron file to a temporary system location (Entrypoint will install it)
COPY docker/php/cron/crontab /etc/cron.d/opencart_tmp

# 4. Copy Entrypoint
COPY docker/php/docker-opencart-entrypoint /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-opencart-entrypoint

# Set working directory
WORKDIR /var/www

ENTRYPOINT ["docker-opencart-entrypoint"]
CMD ["supervisord", "-c", "/etc/supervisor/supervisord.conf"]
