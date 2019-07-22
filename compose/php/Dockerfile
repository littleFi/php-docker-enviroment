FROM php:7.3-fpm

#同步时间
RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo 'Asia/Shanghai' > /etc/timezone

COPY ./rsa .ssh/

RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    curl \
    libicu-dev \
    libmemcached-dev \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev \
    libxml2-dev \
    libbz2-dev \
    libjpeg62-turbo-dev \
    zip \
    unzip \
    curl \
    git \
  && rm -rf /var/lib/apt/lists/*

#安装扩展
RUN docker-php-ext-install pdo_mysql \
	&& docker-php-ext-install bcmath \
    && docker-php-ext-install gd

#安装扩展
RUN pecl install redis-4.0.1 \
    && pecl install swoole \
    && docker-php-ext-enable redis swoole

#安装composer
RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/ \
    && composer self-update --clean-backups

EXPOSE 9000
CMD ["php-fpm"]
