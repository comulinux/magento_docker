FROM phpdockerio/php7-fpm:latest

WORKDIR "/application"

#BASE SYTEM
RUN apt-get update \
    && apt-get -y --no-install-recommends install curl php7.0-mysql php7.0-mbstring php7.0-xdebug php7.0-bcmath \
    php7.0-gd php7.0-intl php7.0-soap php7.0-xml php7.0-mcrypt php-codesniffer phpunit \
    php7.0-zip php7.0-xml php7.0-soap php7.0-intl php7.0-curl sudo cron \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*
    
#ADDS USER
RUN useradd magento -g www-data -s /bin/bash && usermod -a -G users,www-data,sudo magento

#COMPOSER
ENV COMPOSER_HOME /var/www/.composer

RUN curl -sS https://getcomposer.org/installer | php -- \
    --install-dir=/usr/bin \
    --filename=composer

RUN ln -s /application/bin/magento /usr/bin