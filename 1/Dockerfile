FROM php:8.4-fpm AS base
LABEL org.opencontainers.image.source="https://github.com/nickdown/php-docker-images"

# Install dependencies
RUN apt-get update && apt-get install -y \
    git unzip curl libsqlite3-dev libzip-dev zip gnupg \
    nginx nodejs npm supervisor cron

RUN pecl install --onlyreqdeps --force redis \
&& rm -rf /tmp/pear \
&& docker-php-ext-enable redis

# Symlink nodejs if necessary
RUN [ -f /usr/bin/node ] || ln -s /usr/bin/nodejs /usr/bin/node

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Create app dir and ensure logs directory exists
RUN mkdir -p /var/www/html/storage/logs && \
    chown -R www-data:www-data /var/www/html && \
    chmod -R 775 /var/www/html/storage

# Copy Nginx config
COPY nginx.conf /etc/nginx/sites-available/default

# Copy Supervisor config
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Set up cron
COPY cron /etc/cron.d/cron
RUN chmod 0644 /etc/cron.d/cron && \
    crontab /etc/cron.d/cron && \
    touch /var/www/html/storage/logs/scheduler.log && \
    chown www-data:www-data /var/www/html/storage/logs/scheduler.log

# Entrypoint runs supervisor (which manages php-fpm + nginx + cron)
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]
