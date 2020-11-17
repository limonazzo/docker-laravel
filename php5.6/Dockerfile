## docker build -t php:5.6 .
FROM php:5.6.37-apache-jessie

RUN apt-get update

RUN apt-get install -y libbz2-dev
RUN apt-get install -y zlib1g-dev
RUN apt-get install -y libfreetype6-dev
RUN apt-get install -y libjpeg62-turbo-dev
RUN apt-get install -y libpng-dev
RUN apt-get install -y libicu-dev
RUN apt-get install -y sendmail
RUN apt-get install -y libxml2-dev
RUN apt-get install -y vim
RUN apt-get install -y git
RUN apt-get install -y zip
RUN apt-get install -y unzip
RUN apt-get install -y sudo

RUN docker-php-ext-install bcmath
RUN docker-php-ext-install bz2
RUN docker-php-ext-install calendar
RUN docker-php-ext-install ctype
RUN docker-php-ext-install fileinfo
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install gd
RUN docker-php-ext-install intl
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install soap
RUN docker-php-ext-install zip
RUN docker-php-ext-install dba
RUN docker-php-ext-install mssql
RUN docker-php-ext-install mysql
RUN docker-php-ext-install pdo
RUN docker-php-ext-install pdo_dblib
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install pdo_sqlite

RUN apt install -y libmcrypt-dev
RUN docker-php-ext-configure mcrypt
RUN docker-php-ext-install mcrypt

COPY dockerfiles/php.ini /usr/local/etc/php/
COPY dockerfiles/default-ssl.conf /etc/apache2/sites-available
COPY dockerfiles/000-default.conf /etc/apache2/sites-available
COPY dockerfiles/xdebug.ini /tmp/xdebug.ini

RUN openssl req -x509 -nodes -days 3650 -newkey rsa:2048 -keyout /etc/ssl/private/ssl-cert-snakeoil.key -out /etc/ssl/certs/ssl-cert-snakeoil.pem -subj "/C=MX/ST=Mexico/L=MExico/O=Security/OU=Desarrollo/CN=limonazzo.com"
RUN a2enmod rewrite
RUN a2ensite default-ssl
RUN a2enmod ssl

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php
RUN cp composer.phar /usr/bin/composer

WORKDIR /var/www/html

EXPOSE 80
EXPOSE 443