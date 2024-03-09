FROM php:8-apache
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
libc-client-dev \
libkrb5-dev \
git \
nodejs \
openssh-client \
unzip \
npm --no-install-recommends \
&& rm -r /var/lib/apt/lists/*

RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl
RUN docker-php-ext-configure gd --with-freetype=/usr/include/ \
--with-jpeg=/usr/include/
RUN docker-php-ext-install -j$(nproc) \
mysqli exif zip dba bz2 calendar bcmath gettext gmp intl pdo pdo_mysql pspell gd \
#mysqli exif mbstring zip dba bz2 calendar bcmath gettext gmp intl pdo pdo_mysql pspell gd \
#imap iconv mcrypt soap sockets sysvmsg sysvsem sysvshm wddx xmlrpc xsl
soap sysvmsg sysvsem sysvshm
#RUN docker-php-ext-install -j$(nproc) sockets
#RUN docker-php-ext-install -j$(nproc) xsla

RUN pecl install imagick && docker-php-ext-enable imagick
RUN pecl install -o -f redis \
&& rm -rf /tmp/pear \
&& docker-php-ext-enable redis


RUN a2enmod rewrite
ENV APACHE_RUN_USER stephane
ENV APACHE_RUN_GROUP stephane
RUN addgroup --gid 1000 stephane \
	&& adduser --uid 1000 --gid 1000 --shell /bin/bash --home /home/stephane --gecos 'My Apache User' --disabled-password stephane \
        && chown -R "$APACHE_RUN_USER:$APACHE_RUN_GROUP" /var/lock/apache2 /var/run/apache2

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
&& php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
&& php composer-setup.php --install-dir=/usr/local/bin \
&& php -r "unlink('composer-setup.php');"

RUN echo 'alias composer="php /usr/local/bin/composer.phar"' >> ~/.bashrc \
	&& echo 'alias ll="ls -l"' >> ~/.bashrc \
	&& echo 'alias lr="ls -lrt"' >> ~/.bashrc \
	&& echo 'alias tf="tail -f "' >> ~/.bashrc \
	&& cp ~/.bashrc /home/stephane/.bashrc \
	&& echo 'cd /var/www/html' >> /home/stephane/.bashrc \
	&& su - stephane -c "git config --global user.name 'Stéphane Burlet'" \
	&& su - stephane -c "git config --global user.email stephtkd@yahoo.fr"

#COPY php.ini-production /usr/local/etc/php/php.ini
#COPY 000-default.conf /etc/apache2/sites-available/000-default.conf

#WORKDIR /var/www/html
