## docker build -t php:7.1 .
FROM php:7.1.33-apache-stretch

RUN apt-get update

## Extensions Dependencies

RUN apt-get install -y libmcrypt-dev
RUN apt-get install -y libfreetype6-dev
RUN apt-get install -y libjpeg62-turbo-dev
RUN apt-get install -y libpng-dev
RUN apt-get install -y apt-utils
RUN apt-get install -y libicu-dev
RUN apt-get install -y libcurl4-openssl-dev
RUN apt-get install -y libbz2-dev
RUN apt-get install -y zlib1g-dev
RUN apt-get install -y libxml2-dev
RUN apt-get install -y libzip-dev
RUN apt-get install -y zip
RUN apt-get install -y unzip
RUN apt-get install -y libwebp-dev
RUN apt-get install -y libxpm-dev
RUN apt-get install -y libxslt1-dev

## PHP Extensions Laravel Requirements
RUN docker-php-ext-install mcrypt
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install ctype
RUN docker-php-ext-install fileinfo
RUN docker-php-ext-install json
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install intl


## PHP Extensions Common
RUN docker-php-ext-install bz2
RUN docker-php-ext-install curl
RUN docker-php-ext-install calendar
RUN docker-php-ext-configure gd \
    --with-freetype-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ \
    --with-xpm-dir=/usr/include/ \
    --with-webp-dir=/usr/include/
RUN docker-php-ext-install gd
RUN docker-php-ext-install soap
RUN docker-php-ext-install sockets
RUN docker-php-ext-install xsl
RUN docker-php-ext-install zip

## Custom Configuration  (php.ini , apache, xdebug)
COPY dockerfiles/php.ini /usr/local/etc/php/
COPY dockerfiles/default-ssl.conf /etc/apache2/sites-available
COPY dockerfiles/000-default.conf /etc/apache2/sites-available

## Fake SSL
RUN openssl req -x509 \
    -nodes -days 3650 \
    -newkey rsa:2048 \
    -keyout /etc/ssl/private/ssl-cert-snakeoil.key \
    -out /etc/ssl/certs/ssl-cert-snakeoil.pem \
    -subj "/C=MX/ST=Mexico/L=Mexico/O=Security/OU=Develop/CN=limonazzo.com"
RUN a2enmod rewrite
RUN a2ensite default-ssl
RUN a2enmod ssl

## Install dev tools
RUN apt-get install -y vim
RUN apt-get install -y git
RUN apt-get install -y sudo

## Install Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php
RUN rm composer-setup.php
RUN mv composer.phar /usr/bin/composer

RUN chown -R www-data:www-data /var/www/

WORKDIR /var/www/html

EXPOSE 80
EXPOSE 443