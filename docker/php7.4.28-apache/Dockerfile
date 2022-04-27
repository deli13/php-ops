FROM php:7.4.28-apache

WORKDIR /var/www/html

RUN apt-get update && \
    apt-get install -y vim git nano curl libmcrypt-dev libfreetype6-dev libjpeg-dev libicu-dev  libxml2-dev zip


RUN apt-get update && \
    pecl channel-update pecl.php.net && \
    pecl install apcu 
    # compile Redis with igbinary support
RUN docker-php-ext-install bcmath sockets  \
    && docker-php-ext-install -j$(nproc) iconv intl xml soap opcache pdo pdo_mysql mysqli \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-source delete

RUN rm -r /tmp/* /var/cache/* 

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php -r "if (hash_file('sha384', 'composer-setup.php') === '906a84df04cea2aa72f40b5f787e49f22d4c2f19492ac310e8cba5b96ac8b64115ac402c8cd292b8a03482574915d1a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer

RUN a2enmod rewrite && service apache2 restart

COPY default.conf /etc/apache2/sites-enabled/000-default.conf

