FROM php:7.4-fpm
MAINTAINER mantis "tangchunlinit@gmail.com"

# set timezone.
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# update dependency package & install php core extension.
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libzip-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install zip \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install opcache \
    && docker-php-ext-install mysqli

# copy extension pkg to the home dir.
COPY ./libs/redis-5.1.1.tgz /tmp/redis.tgz

# install redis extension.
RUN pecl install /tmp/redis.tgz && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini

# rm ext tar pkg.
RUN rm -rf /tmp/redis.tgz \

# set word dir.
WORKDIR /init

# permission mod.
RUN usermod -u 1000 www-data