FROM php:5.6-fpm

MAINTAINER Yu Shi <mynsa04@ya.ru>

imagemagick \
    graphicsmagick \
    libmemcached-dev \
    zip \
    unzip \
    curl \
    --no-install-recommends \
    && rm -r /var/lib/apt/lists/*

# GD
RUN buildRequirements="libpng12-dev libjpeg-dev libfreetype6-dev" \
    && apt-get update && apt-get install -y ${buildRequirements} \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/lib \
    && docker-php-ext-install gd \
    && apt-get purge -y ${buildRequirements} \
    && rm -rf /var/lib/apt/lists/*

# Imagick
RUN runtimeRequirements="libmagickwand-6.q16-dev --no-install-recommends" \
    && apt-get update && apt-get install -y ${runtimeRequirements} \
    && ln -s /usr/lib/x86_64-linux-gnu/ImageMagick-6.8.9/bin-Q16/MagickWand-config /usr/bin/ \
    && pecl install imagick-3.4.0RC6 \
    && echo "extension=imagick.so" > /usr/local/etc/php/conf.d/ext-imagick.ini \
    && rm -rf /var/lib/apt/lists/*

# Opcache
RUN docker-php-ext-install opcache

# Zip
RUN docker-php-ext-install zip

# Install extensions using the helper script provided by the base image
RUN docker-php-ext-install pdo_mysql

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# Set timezone
RUN rm /etc/localtime
RUN ln -s /usr/share/zoneinfo/America/Mexico_City /etc/localtime
RUN "date"

# Type docker-php-ext-install to see available extensions
RUN docker-php-ext-install pdo pdo_mysql

# install xdebug
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug
RUN echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "display_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini


RUN echo 'alias sf="php app/console"' >> ~/.bashrc
RUN echo 'alias sf3="php bin/console"' >> ~/.bashrc

WORKDIR /var/www/symfony
