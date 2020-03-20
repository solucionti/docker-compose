FROM php:7.2.10-fpm

RUN apt-get update && apt-get -y --no-install-recommends install libxml2-dev pkg-config libssl-dev libpng-dev libjpeg-dev libmemcached-dev zlib1g-dev libmemcached-tools libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev autoconf pkg-config libssl-dev msmtp mariadb-client nano git
#RUN pecl install mongodb-1.2.2
#RUN echo "extension=mongodb.so" >> /usr/local/etc/php/conf.d/mongodb.ini

# Install required PHP extensions.
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install zip
RUN docker-php-ext-install exif
RUN docker-php-ext-install -j$(nproc) iconv
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/freetype2  --with-png-dir=/usr/include  --with-jpeg-dir=/usr/include
RUN docker-php-ext-install gd
RUN docker-php-ext-install pdo pdo_mysql
RUN docker-php-ext-install mysqli
RUN docker-php-ext-configure mbstring --enable-mbstring 
RUN docker-php-ext-install mbstring 
RUN docker-php-ext-install pcntl zip opcache soap
RUN docker-php-ext-enable opcache
RUN docker-php-ext-configure intl
RUN docker-php-ext-install intl
RUN docker-php-ext-install mysqli
RUN docker-php-ext-enable mysqli
RUN pecl install mcrypt-1.0.1 && docker-php-ext-enable mcrypt

# http://docs.drush.org/en/master/install/
# Install Composer for installing Drush
RUN curl -sS https://getcomposer.org/installer | php && \
mv composer.phar /usr/local/bin/composer && \
ln -s /usr/local/bin/composer /usr/bin/composer

## Install Drush (latest)
#RUN apt-get -y --no-install-recommends install git && \
#git clone https://github.com/drush-ops/drush.git /usr/local/src/drush && \
#cd /usr/local/src/drush && \
#git checkout 7.x && \
#ln -s /usr/local/src/drush/drush /usr/bin/drush && \
#composer install

#Install DrupalConsole
RUN curl https://drupalconsole.com/installer -L -o drupal.phar && \
mv drupal.phar /usr/local/bin/drupal && \
chmod +x /usr/local/bin/drupal && \
drupal self-update

# Clean up apt
RUN apt-get clean && \
rm -r /var/lib/apt/lists/* && \

# Extract php source files files for use downstream
tar -C "/usr/src" -xvf "/usr/src/php.tar.xz" && \
mv /usr/src/php-$PHP_VERSION /usr/src/php



COPY php.ini /usr/local/etc/php/conf.d/php.ini


