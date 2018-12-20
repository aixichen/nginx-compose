from php:5.6.38-fpm-alpine
run mkdir -p /data/wwwroot/api
ENV PHPREDIS_VERSION 3.1.3
ENV PHPSWOOLE_VERSION 1.10.5
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
    &&apk update
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
WORKDIR /data/wwwroot/api
user root
