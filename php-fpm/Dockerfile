FROM php:7.1-fpm

WORKDIR "/application"

RUN apt-get update \
    && apt-get -y --no-install-recommends install curl \
    libicu-dev \
    libmemcached-dev \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev \
    libxslt-dev \
    libxml2-dev \
    libbz2-dev \
    libjpeg62-turbo-dev \
    curl \
    git \
  && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-configure bcmath --enable-bcmath \
    && docker-php-ext-configure pcntl --enable-pcntl \
    && docker-php-ext-configure pdo_mysql --with-pdo-mysql \
    && docker-php-ext-configure pdo_pgsql --with-pgsql \
    && docker-php-ext-configure mbstring --enable-mbstring \
    && docker-php-ext-configure soap --enable-soap \
    && docker-php-ext-install \
        bcmath \
        intl \
        mbstring \
        mcrypt \
        mysqli \
        pcntl \
        pdo_mysql \
        pdo_pgsql \
        soap \
        sockets \
        zip \
        xsl \
  && docker-php-ext-configure gd \
    --enable-gd-native-ttf \
    --with-jpeg-dir=/usr/lib \
    --with-freetype-dir=/usr/include/freetype2 && \
    docker-php-ext-install gd 

# Install xDebug, if enabled
ARG INSTALL_XDEBUG=true
RUN if [ ${INSTALL_XDEBUG} = true ]; then \
    # Install the xdebug extension
    pecl install xdebug && \
    docker-php-ext-enable xdebug \
;fi

# Composer
ENV COMPOSER_HOME /var/www/.composer

RUN curl -sS https://getcomposer.org/installer | php -- \
    --install-dir=/usr/bin \
    --filename=composer

RUN chown -R www-data:www-data /var/www/

RUN mkdir -p $COMPOSER_HOME/cache

VOLUME $COMPOSER_HOME

# User
RUN useradd magento -g www-data -s /bin/bash && usermod -a -G users,www-data,sudo magento \
    && ln -s /application/bin/magento /usr/bin \
    &&  rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*