from php:5.6.38-fpm-alpine
run mkdir -p /data/wwwroot/im
ENV PHPREDIS_VERSION 3.1.3
ENV PHPSWOOLE_VERSION 1.10.5
ENV TIMEZONE Asia/Shanghai
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
    &&apk update
RUN apk add tzdata git ansible && ln -snf /usr/share/zoneinfo/$TIMEZONE /etc/localtime && echo $TIMEZONE > /etc/timezone

RUN docker-php-ext-install mbstring opcache pdo pdo_mysql

#添加扩展 gd zip
RUN apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev && \
    docker-php-ext-configure gd \
      --with-gd \
      --with-freetype-dir=/usr/include/ \
      --with-png-dir=/usr/include/ \
      --with-jpeg-dir=/usr/include/ \
      --with-zlib-dir=/usr && \
        NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
        docker-php-ext-install -j${NPROC} gd zip && \

        apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev
#添加mcrypt
RUN apk add libmcrypt-dev \
&& docker-php-ext-install mcrypt
RUN curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz \
&& curl -L -o /tmp/swoole.tar.gz https://github.com/swoole/swoole-src/archive/v$PHPSWOOLE_VERSION.tar.gz \    
&& tar xfz /tmp/redis.tar.gz \
    && rm -r /tmp/redis.tar.gz \
    && mkdir -p /usr/src/php/ext \
    && mv phpredis-$PHPREDIS_VERSION /usr/src/php/ext/redis \ 
    && tar xfz /tmp/swoole.tar.gz \
    && rm -r /tmp/swoole.tar.gz \
    && mv swoole-src-$PHPSWOOLE_VERSION /usr/src/php/ext/swoole \
    && docker-php-ext-install redis \
    && docker-php-ext-install swoole \
    && rm -rf /usr/src/php
add php.ini /usr/local/etc/php/php.ini
add restart /data/wwwroot/im/restart
WORKDIR /data/wwwroot/im
RUN apk add bash
RUN chmod +x /data/wwwroot/im/restart
CMD ["/data/wwwroot/im/restart"]
user root
