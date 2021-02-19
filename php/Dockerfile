FROM php:7.3-apache 

RUN apt-get -y update
RUN apt-get -y install curl nano wget



RUN apt-get install -y \
    libbz2-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libxpm-dev \
    libvpx-dev \
    libmcrypt-dev \
    libmemcached-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

RUN docker-php-ext-install \
        bz2 \
        gettext \
        mbstring \
        mysqli \
        opcache \
        shmop \
        sockets \
        sysvmsg \
        sysvsem \
        sysvshm 

RUN a2enmod rewrite
COPY 000-default.conf /etc/apache2/sites-available/000-default.conf

RUN docker-php-ext-install mysqli