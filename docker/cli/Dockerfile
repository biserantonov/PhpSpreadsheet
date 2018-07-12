FROM php:7.1-cli

ARG DEBIAN_FRONTEND=noninteractive

ENV COMPOSER_HOME /composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV PATH /app/vendor/bin:/composer/vendor/bin:$PATH

WORKDIR /var/tmp

# install source dependancies
RUN apt-get update && apt-get install -y \
    git \
    imagemagick \
    libfreetype6-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    libssl-dev \
    libzip-dev \
    zlib1g-dev \
    && rm -rf /var/lib/apt/lists/*

#RUN mkdir /usr/include/freetype2/freetype && ln -s /usr/include/freetype2/freetype.h /usr/include/freetype2/freetype/freetype.h

# install php extensions
RUN pecl channel-update pecl.php.net \
    && docker-php-ext-configure gd --with-jpeg-dir --with-freetype-dir --enable-gd-native-ttf \
    && docker-php-ext-install mcrypt bcmath zip gd
#    && pecl install xdebug && docker-php-ext-enable xdebug

RUN groupadd -r -g 1000 appuser && useradd -r -m -u 1000 -g appuser appuser

# install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && chown -R appuser:appuser /composer \
    && chown appuser:appuser /usr/local/bin/composer

USER appuser
WORKDIR /app
