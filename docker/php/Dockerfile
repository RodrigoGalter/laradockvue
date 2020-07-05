FROM php:7.4-fpm

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y \
    locales \
    locales-all \
    supervisor \
    cron \
    ssh\
    zip \
    mcrypt \
    libsodium-dev \
    libicu-dev \
    libpng-dev \
    libjpeg62-turbo-dev \
    libzip-dev \
    vim && \
    #pecl
    pecl install redis && \
    # Extensions
    docker-php-ext-configure intl && \
    docker-php-ext-configure gd --with-jpeg && \
    docker-php-ext-install pcntl bcmath pdo_mysql mysqli gd intl && \
    # Cron
    echo "*/1 * * * * www-data /var/www/html/artisan schedule:run >> /var/www/html/storage/logs/api-schedule.log 2>&1" >> /etc/crontab && \
    apt-get autoclean -y && \
    apt-get autoremove -y

# Supervisor
COPY supervisor /etc/supervisor/conf.d

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    chmod +x /usr/local/bin/composer

WORKDIR /var/www/html

CMD /usr/sbin/cron && /usr/bin/supervisord && php-fpm