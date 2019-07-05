FROM php:7.2-fpm
MAINTAINER qingkong

# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 更新安装依赖包和PHP核心拓展
RUN apt-get update && apt-get install -y \
        git \
        vim \
        procps\
        curl \
        wget \
        cron \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
		libssl-dev \
		libmcrypt-dev \
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd \
        && docker-php-ext-install zip \
        && docker-php-ext-install pdo_mysql \
        && docker-php-ext-install opcache \
        && docker-php-ext-install mysqli \
        && docker-php-ext-install pcntl \
        && rm -r /var/lib/apt/lists/*

# pecl 安装mcrypt扩展，兼容老代码
RUN pecl install mcrypt-1.0.2 && echo "extension=mcrypt.so" > /usr/local/etc/php/conf.d/mcrypt.ini

# pecl 安装 Redis
RUN pecl install redis && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini

# pecl 安装 MongoDb
RUN pecl install mongodb && echo "extension=mongodb.so" > /usr/local/etc/php/conf.d/mongodb.ini

# pecl 安装swoole
RUN pecl install swoole && echo "extension=swoole.so" > /usr/local/etc/php/conf.d/swoole.ini

# pecl 安装xdebug
RUN pecl install xdebug && echo "zend_extension=xdebug.so" > /usr/local/etc/php/conf.d/xdebug.ini

# 安装 Swoole
#RUN cd /home \
#    && git clone https://gitee.com/swoole/swoole.git \
#    && cd swoole \
#    && phpize \
#    && ./configure --enable-openssl -with-php-config=/usr/local/bin/php-config \
#    && make \
#    && make install \
#    && echo "extension=swoole.so" > /usr/local/etc/php/conf.d/swoole.ini

# 安装 Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH

WORKDIR /data

# 写权限
RUN usermod -u 1000 www-data
