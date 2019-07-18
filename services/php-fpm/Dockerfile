#soterea 平台 & app 服务端php环境

FROM php:7.2-fpm
MAINTAINER qingkong

# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 更换国内源

RUN sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
RUN apt-get clean
RUN apt-get update 


#安装依赖
RUN apt-get install -y \        
    git \
    vim \
    procps\
    curl \
    wget \
    cron \
    supervisor\
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \    
    libssl-dev \
    libmcrypt-dev

RUN docker-php-ext-install zip \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install opcache \
    && docker-php-ext-install mysqli \
    && docker-php-ext-configure gd --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd


# 安装 Redis
RUN pecl install redis && docker-php-ext-enable redis

# 安装 MongoDb
RUN pecl install mongodb && docker-php-ext-enable mongodb

# 安装swoole
RUN pecl install swoole && docker-php-ext-enable swoole

# 安装xdebug 生产环境注释掉
#RUN pecl install xdebug && docker-php-ext-enable xdebug

# pecl 安装mcrypt扩展，兼容老代码
#RUN pecl install mcrypt-1.0.2 && docker-php-ext-enable mcrypt

# 安装 Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH

#php.ini
RUN cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini

WORKDIR /data

# 写权限
RUN usermod -u 1000 www-data
