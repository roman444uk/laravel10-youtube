FROM library/php:8.2-fpm

RUN apt-get update && apt-get install -y \
        curl \
        wget \
        git \
        nodejs \
        npm \
        libfreetype6-dev \
        libonig-dev \
        libpq-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libzip-dev \
		libmagickwand-dev --no-install-recommends \
		libmemcached-dev \
        sendmail \
        nano \
        supervisor \
	&& pecl install imagick \
    && pecl install redis \
    && pecl install xdebug \
    && docker-php-ext-install -j$(nproc) intl iconv mbstring pdo pgsql pdo_pgsql zip bcmath exif \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install opcache \
    && docker-php-ext-configure pcntl --enable-pcntl \
    && docker-php-ext-install pcntl \
	&& docker-php-ext-enable imagick \
    && docker-php-ext-enable redis \
    && docker-php-ext-enable intl \
    && docker-php-ext-enable xdebug

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
RUN chmod +x /usr/local/bin/install-php-extensions && install-php-extensions mcrypt

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN chown -R www-data:www-data .

ADD ./php.ini /usr/local/etc/php/conf.d/custom.ini
ADD ./php-xdebug.ini /usr/local/etc/php/conf.d/custom-xdebug.ini

WORKDIR /var/www

CMD ["php-fpm"]
