## docker build -t php:7.4 .
FROM php:7.4.12-apache-buster

RUN apt-get update

## Extensions Dependencies
RUN apt-get install -y apt-utils
RUN apt-get install -y libicu-dev
RUN apt-get install -y libcurl4-openssl-dev
RUN apt-get install -y libbz2-dev
RUN apt-get install -y zlib1g-dev
RUN apt-get install -y libfreetype6-dev
RUN apt-get install -y libjpeg62-turbo-dev
RUN apt-get install -y libxml2-dev
RUN apt-get install -y libzip-dev
RUN apt-get install -y zip
RUN apt-get install -y unzip
RUN apt-get install -y libwebp-dev
RUN apt-get install -y libxpm-dev
RUN apt-get install -y libxslt1-dev

## PHP Extensions Laravel Requirements
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
    --with-webp \
    --with-freetype \
    --with-xpm \
    --with-jpeg
RUN docker-php-ext-install gd
RUN docker-php-ext-install soap
RUN docker-php-ext-install sockets
RUN docker-php-ext-install xsl
RUN docker-php-ext-install zip

## PHP Extensions 
# RUN docker-php-ext-install dba
# RUN docker-php-ext-install enchant
# RUN docker-php-ext-install exif
# RUN docker-php-ext-install ffi
# RUN docker-php-ext-install filter
# RUN docker-php-ext-install ftp
# RUN docker-php-ext-install gettext
# RUN docker-php-ext-install gmp
# RUN docker-php-ext-install hash
# RUN docker-php-ext-install iconv
# RUN docker-php-ext-install imap
# RUN docker-php-ext-install intl
# RUN docker-php-ext-install ldap
# RUN docker-php-ext-install oci8
# RUN docker-php-ext-install odbc
# RUN docker-php-ext-install opcache
# RUN docker-php-ext-install pcntl
# RUN docker-php-ext-install pdo
# RUN docker-php-ext-install pdo_dblib
# RUN docker-php-ext-install pdo_firebird
# RUN docker-php-ext-install pdo_mysql
# RUN docker-php-ext-install pdo_oci
# RUN docker-php-ext-install pdo_odbc
# RUN docker-php-ext-install pdo_pgsql
# RUN docker-php-ext-install pdo_sqlite
# RUN docker-php-ext-install pgsql
# RUN docker-php-ext-install phar
# RUN docker-php-ext-install posix
# RUN docker-php-ext-install pspell
# RUN docker-php-ext-install readline
# RUN docker-php-ext-install reflection
# RUN docker-php-ext-install session
# RUN docker-php-ext-install shmop
# RUN docker-php-ext-install simplexml
# RUN docker-php-ext-install snmp
# RUN docker-php-ext-install sodium
# RUN docker-php-ext-install spl
# RUN docker-php-ext-install sysvmsg
# RUN docker-php-ext-install sysvsem
# RUN docker-php-ext-install sysvshm
# RUN docker-php-ext-install tidy
# RUN docker-php-ext-install xmlreader
# RUN docker-php-ext-install xmlrpc
# RUN docker-php-ext-install xmlwriter
# RUN docker-php-ext-install zend_test

## Custom Configuration  (php.ini , apache, xdebug)
COPY dockerfiles/php.ini /usr/local/etc/php/
COPY dockerfiles/default-ssl.conf /etc/apache2/sites-available
COPY dockerfiles/000-default.conf /etc/apache2/sites-available
COPY dockerfiles/xdebug.ini /tmp/xdebug.ini

## Xdebug config
## XDEBUG
RUN pecl install xdebug
ENV XDEBUGINI_PATH=/usr/local/etc/php/conf.d/xdebug.ini
RUN echo "zend_extension="`find /usr/local/lib/php/extensions/ -iname 'xdebug.so'` > $XDEBUGINI_PATH
RUN cat /tmp/xdebug.ini >> $XDEBUGINI_PATH

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

# ##### FrontEndTools:

# Node 
RUN curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
RUN apt-get install -y nodejs
RUN apt-get install gcc g++ make

## YARN
RUN curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install yarn

# ## SASS
RUN npm install -g sass

# ## COMPASS
RUN apt-get install -y ruby-full
RUN gem update --system
RUN gem install compass

## GULP
RUN npm install gulp-cli -g

## GRUNT
RUN npm install grunt-cli -g

## webpack
RUN npm install webpack -g

## angular
# RUN npm install @angular/cli -g

## brunch
RUN npm install -g brunch

## PArcel
RUN npm install -g parcel-bundler 


RUN chown -R www-data:www-data /var/www/

WORKDIR /var/www/html

EXPOSE 80
EXPOSE 443