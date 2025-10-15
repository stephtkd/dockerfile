FROM php:8.1-apache
RUN apt-get update \
&& apt-get install -y \
libpng-dev \
libjpeg-dev \
libfreetype6-dev \
libmcrypt-dev \
libbz2-dev \
libgmp-dev \
libzip-dev \
libicu-dev \
git \
nodejs \
npm \
unzip \
--no-install-recommends \
&& rm -r /var/lib/apt/lists/*
# libc-client-dev \
# libkrb5-dev \
# libpspell-dev \
# libmagickwand-dev \
# libxslt-dev \
# memcached \
# libmemcached-tools \
# aptitude \
# libfreetype6-dev \
# libjpeg62-turbo-dev \
# openssh-client \

# RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
&& docker-php-ext-install -j$(nproc) \
mysqli exif zip dba bz2 calendar bcmath gettext gmp intl pdo pdo_mysql gd 
#mysqli exif mbstring zip dba bz2 calendar bcmath gettext gmp intl pdo pdo_mysql pspell gd \
#imap iconv mcrypt soap sockets sysvmsg sysvsem sysvshm wddx xmlrpc xsl
# soap sysvmsg sysvsem sysvshm
#RUN docker-php-ext-install -j$(nproc) xsla

#RUN pecl install imagick && docker-php-ext-enable imagick
# RUN pecl install -o -f redis \
# && rm -rf /tmp/pear \
# && docker-php-ext-enable redis


RUN a2enmod rewrite
ENV APACHE_RUN_USER=stephane
ENV APACHE_RUN_GROUP=stephane
RUN addgroup --gid 1000 stephane \
	&& adduser --uid 1000 --gid 1000 --shell /bin/bash --home /home/stephane --gecos 'My Apache User' --disabled-password stephane \
        && chown -R "$APACHE_RUN_USER:$APACHE_RUN_GROUP" /var/lock/apache2 /var/run/apache2

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
&& php -r "if (hash_file('sha384', 'composer-setup.php') === 'ed0feb545ba87161262f2d45a633e34f591ebb3381f2e0063c345ebea4d228dd0043083717770234ec00c5a9f9593792') { echo 'Installer verified'.PHP_EOL; } else { echo 'Installer corrupt'.PHP_EOL; unlink('composer-setup.php'); exit(1); }" \
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
