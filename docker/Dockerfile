FROM php:7.3-apache

RUN apt-get update && apt-get -y install \
    # mcrypt
    mc \
    vim \
    git \
    # a reference for all the packages needed to compile a Debian package (c, c++, ..)
    build-essential \
    # lite, dynamic programming language
    tcl8.6 \
    # command line tool > providing libcurl
    curl \
    libcurl4-openssl-dev \
    # data compression lib
    zlib1g-dev \
    # library for configuring and customizing font access
    fontconfig \
    # secure shell
    ssh \
    # retrieving files using HTTP, HTTPS, FTP and FTPS
    wget \
    # encrypt data and to create digital signatures
    gnupg2 \
    libzip-dev \
    --no-install-recommends

#---------------#
# Apache config #
#---------------#

RUN a2enmod rewrite &&\
	a2enmod headers &&\
	a2enmod ssl

RUN echo "ServerName dp.dev" >> /etc/apache2/apache2.conf

ADD ./hosts/* /etc/apache2/sites-available/

RUN cd /etc/apache2/sites-available/ &&\
    a2dissite * &&\
    a2ensite dp.conf

#--------------------#
# PHP install/config #
#--------------------#

RUN pecl install xdebug
RUN docker-php-ext-install pdo pdo_mysql curl mbstring zip mysqli
RUN docker-php-ext-enable xdebug

RUN echo "date.timezone = 'UTC'" >> /usr/local/etc/php/php.ini
RUN echo "error_display = On" >> /usr/local/etc/php/php.ini
RUN echo "error_reporting = E_ALL & ~E_NOTICE" >> /usr/local/etc/php/php.ini
RUN echo "file_uploads = On" >> /usr/local/etc/php/php.ini

RUN echo "xdebug.remote_port = 9000" >> /usr/local/etc/php/php.ini
RUN echo "xdebug.remote_enable = 1" >> /usr/local/etc/php/php.ini
RUN echo "xdebug.remote_connect_back = 1" >> /usr/local/etc/php/php.ini
RUN echo "xdebug.idekey = 'PHPSTORM'" >> /usr/local/etc/php/php.ini
RUN echo "xdebug.remote_log = '/tmp/xdebug.log'" >> /usr/local/etc/php/php.ini
RUN echo "xdebug.remote_timeout = 20000" >> /usr/local/etc/php/php.ini

#----------------#
# General config #
#----------------#

RUN mkdir /var/www/php_upload
RUN chmod -R 777 /var/www/php_upload

RUN mkdir /var/www/php_upload/sessions
RUN chmod -R 777 /var/www/php_upload/sessions

# usermod is used to modify an user account
RUN usermod -s /bin/bash www-data

WORKDIR /var/www/sources
