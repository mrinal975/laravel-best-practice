FROM php:8.1-fpm
WORKDIR /var/www/laravel-best-practice
RUN chown -R www-data:www-data /var/www/laravel-best-practice
RUN apt-get update && apt-get install -y \
    git \
    libzip-dev \
    zip \
    unzip \
    ldap-utils \
    libldap2-dev \
    curl \
    libpng-dev \
    supervisor \
    procps


RUN apt-get update && \
    apt-get install -y libxml2-dev
RUN docker-php-ext-install soap

#upload
RUN echo "file_uploads = On\n" \
    "memory_limit = 500M\n" \
    "upload_max_filesize = 500M\n" \
    "post_max_size = 500M\n" \
    "max_execution_time = 6000\n" \
    > /usr/local/etc/php/conf.d/uploads.ini


RUN docker-php-ext-install pdo_mysql zip ldap exif bcmath
RUN docker-php-ext-configure zip \
    && docker-php-ext-configure bcmath --enable-bcmath
# RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN docker-php-ext-install gd

RUN curl --silent --show-error https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer

COPY start-container /usr/local/bin/start-container
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
RUN chmod +x /usr/local/bin/start-container

ENTRYPOINT ["start-container"]
