#消息队列消费者入库

FROM php:7.2.18-cli-alpine3.9

MAINTAINER haixinz

RUN echo 'http://mirrors.aliyun.com/alpine/latest-stable/main/' > /etc/apk/repositories; \
    echo 'http://mirrors.aliyun.com/alpine/latest-stable/community/' >> /etc/apk/repositories

RUN echo /etc/apk/respositories && apk update

RUN apk add --no-cache bash \
    openssh-client \
    wget \
    supervisor \
    curl \
    libcurl \
    augeas-dev \
    openssl-dev \
    ca-certificates \
    dialog \
    autoconf \
    automake \
    make \
    gcc \
    g++ \
    musl-dev \
    linux-headers \
    libmcrypt-dev \
    libpng-dev \
    libc-dev \
    icu-dev \
    pcre-dev \
    zlib-dev \
    libpq \
    libxslt-dev \
    libffi-dev \
    freetype-dev \
    sqlite-dev \
    libjpeg-turbo-dev \
    vim \
    git \
    gnupg \
    gd-dev \
    geoip-dev \
    perl-dev \
    php-pear \
    libtool 

# fix some path error
RUN mkdir -p /usr/src/php/ext

# install composer    
RUN EXPECTED_COMPOSER_SIGNATURE=$(wget -q -O - https://composer.github.io/installer.sig) && \
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php -r "if (hash_file('SHA384', 'composer-setup.php') === '${EXPECTED_COMPOSER_SIGNATURE}') { echo 'Composer.phar Installer verified'; } else { echo 'Composer.phar Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
    php composer-setup.php --install-dir=/usr/bin --filename=composer && \
    php -r "unlink('composer-setup.php');"

#------------------------------------------------------
# install php pdo_mysql mysqli extension
RUN docker-php-ext-install mysqli pdo_mysql

#------------------------------------------------------

# install gd extension

RUN apk add libpng-dev && docker-php-ext-install gd

RUN docker-php-ext-install pdo_mysql mysqli exif fileinfo intl json opcache

RUN git clone https://github.com/edenhill/librdkafka.git && \
    cd librdkafka && \
    ./configure && \
    make && make install


#install kafka
RUN pecl install rdkafka

#install swoole 
RUN pecl install swoole

#install redis
RUN pecl install redis

#install mongodb
RUN pecl install mongodb

# other extension
RUN docker-php-ext-enable redis.so && \
    docker-php-ext-enable swoole.so && \
    docker-php-ext-enable rdkafka.so && \
    docker-php-ext-enable mongodb.so && \


RUN rm -rf /home/librdkafka

RUN cp php.ini-production php.ini

# supervisor config file 
#ADD ./conf/supervisord/supervisord.conf /etc/