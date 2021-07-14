FROM php:7.2-apache
RUN apt-get update \
&& apt-get install -y \
libpng-dev \
aptitude \
libfreetype6-dev \
libjpeg62-turbo-dev \
libmcrypt-dev \
libbz2-dev \
libgmp-dev \
libzip-dev \
libc-client-dev \
libkrb5-dev \
libpspell-dev \
libmagickwand-dev \
libxslt-dev \
memcached \
libmemcached-tools \
git \
nodejs \
npm --no-install-recommends \
&& rm -r /var/lib/apt/lists/*

RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ \
--with-jpeg-dir=/usr/include/
RUN docker-php-ext-install -j$(nproc) \
mysqli exif zip dba bz2 calendar bcmath gettext gmp intl pdo pdo_mysql pspell gd \
#mysqli exif mbstring zip dba bz2 calendar bcmath gettext gmp intl pdo pdo_mysql pspell gd \
#imap iconv mcrypt soap sockets sysvmsg sysvsem sysvshm wddx xmlrpc xsl
imap soap sockets sysvmsg sysvsem sysvshm wddx xmlrpc xsl

RUN pecl install imagick && docker-php-ext-enable imagick
RUN pecl install -o -f redis \
&& rm -rf /tmp/pear \
&& docker-php-ext-enable redis

COPY php.ini /usr/local/etc/php/
COPY 000-default.conf /etc/apache2/sites-available/000-default.conf

COPY composer-setup.php /usr/bin/composer-setup.php
WORKDIR /usr/bin
RUN php /usr/bin/composer-setup.php --version=1.6.0

WORKDIR /var/www/html

RUN a2enmod rewrite
ENV APACHE_RUN_USER stephane
ENV APACHE_RUN_GROUP stephane
RUN addgroup --gid 1000 stephane \
	&& adduser --uid 1000 --gid 1000 --shell /bin/bash --home /home/stephane --gecos 'My Apache User' --disabled-password stephane \
             && chown -R "$APACHE_RUN_USER:$APACHE_RUN_GROUP" /var/lock/apache2 /var/run/apache2

RUN echo 'alias composer="php /usr/bin/composer.phar"' >> ~/.bashrc \
	&& echo 'alias ll="ls -l"' >> ~/.bashrc \
	&& echo 'alias lr="ls -lrt"' >> ~/.bashrc \
	&& echo 'alias tf="tail -f "' >> ~/.bashrc \
	&& cp ~/.bashrc /home/stephane/.bashrc \
	&& echo 'cd /var/www/html' >> /home/stephane/.bashrc \
	&& git config --global user.name "Stéphane Burlet" \
	&& git config --global user.email stephtkd@yahoo.fr

