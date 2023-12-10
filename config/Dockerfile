FROM php:8.2-cli

RUN ln -fs /usr/share/zoneinfo/Europe/Berlin /etc/localtime
RUN apt update && apt install -y tzdata \
    && dpkg-reconfigure --frontend noninteractive tzdata

RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libssl-dev \
    zip \
    unzip

RUN apt-get clean && rm -rf /var/lib/apt/lists/*

RUN pecl install mongodb && docker-php-ext-enable mongodb

RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
RUN mkdir -p /home/$user/.composer && chown -R $user:$user /home/$user

# X Debug
RUN pecl install xdebug
COPY ./config/20-xdebug.ini /usr/local/etc/php/conf.d/20-xdebug.ini
RUN  echo "zend_extension=$(find /usr/local/lib/php -name xdebug.so)" >> /usr/local/etc/php/conf.d/20-xdebug.ini

RUN pecl config-set php_ini /usr/local/etc/php/fpm/php.ini

WORKDIR /var/www/public

CMD ["php", "-S", "0.0.0.0:80"]
