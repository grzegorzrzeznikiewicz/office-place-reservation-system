FROM php:7.3-fpm

WORKDIR /var/www/html/

RUN docker-php-ext-install pdo_mysql

RUN pecl install apcu

RUN apt-get update && \
    apt-get install -y -q --no-install-recommends \
          libzip-dev \
          zlib1g-dev \
          bison \
          libicu-dev \
          libfreetype6-dev \
          libjpeg62-turbo-dev \
          libpng-dev \
          libcurl4-gnutls-dev \
          libbz2-dev \
          libssl-dev \
          libmcrypt-dev \
          libmagickwand-dev \
          apt-transport-https \
          build-essential \
          ca-certificates \
          curl \
          python \
          rsync \
          cron \
          supervisor \
          wget \
          ssh-import-id \
          locales \
          software-properties-common \
          haproxy \
          telnet \
          psmisc

RUN docker-php-ext-install pdo_mysql \
      opcache \
      calendar \
      bcmath \
      zip \
      bz2 \
      intl

# Install GD extension
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install gd

# Install PECL extensions
RUN pecl install xdebug
RUN pecl install mcrypt-1.0.2
RUN pecl install imagick

RUN docker-php-ext-enable mcrypt
RUN docker-php-ext-enable imagick
RUN docker-php-ext-enable apcu

# Install composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('SHA384', 'composer-setup.php') === 'e0012edf3e80b6978849f5eff0d4b4e4c79ff1609dd1e613307e16318854d24ae64f26d17af3ef0bf7cfb710ca74755a') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php --filename=composer \
    && php -r "unlink('composer-setup.php');" \
    && mv composer /usr/local/bin/composer

# Add aliases for xdebug
RUN echo 'alias disablexdebug="rm -rf /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
                               && killall -KILL php-fpm"' >> /root/.bashrc

RUN echo 'alias enablexdebug="echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
                                 && echo "xdebug.remote_port=9000" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
                                 && echo "xdebug.remote_connect_back=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
                                 && echo "xdebug.remote_host=local_host" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
                                 && echo "xdebug.idekey=PHPSTORM" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
                                 && echo "xdebug.max_nesting_level=1000" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
                                 && chmod 666 /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
                                 && docker-php-ext-enable xdebug \
                                 && killall -KILL php-fpm"' >> /root/.bashrc

RUN chown -R www-data:www-data /var/www/html

RUN PATH=$PATH:/var/www/html/vendor/bin:bin