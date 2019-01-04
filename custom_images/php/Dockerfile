ARG version=7.2
FROM php:${version}-fpm
MAINTAINER Navalona Ramanantoanina <job.navds@gmail.com>

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libxml2-dev \
        libpng-dev \
        libxslt1-dev \
        ldap-utils \
        libldap2-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-configure \
        gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
    && docker-php-ext-install -j$(nproc) iconv \
        bcmath gd intl mbstring \
        opcache pdo_mysql soap xsl zip xmlrpc ldap

RUN pecl channel-update pecl.php.net \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && sed -i -e 's/^zend_extension/\;zend_extension/g' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN curl -sS https://getcomposer.org/installer | \
  php -- --install-dir=/usr/local/bin --filename=composer

RUN groupadd -g 1000 app \
 && useradd -g 1000 -u 1000 -d /code -s /bin/bash app


WORKDIR /code
