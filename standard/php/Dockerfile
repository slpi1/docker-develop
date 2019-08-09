# PHP 容器配置
# 通过该文件配置的容器功能比较全面，但体积较大

FROM php:7.2-fpm

RUN docker-php-source extract \

    ## imagick
    && export CFLAGS="$PHP_CFLAGS" CPPFLAGS="$PHP_CPPFLAGS" LDFLAGS="$PHP_LDFLAGS" \
    
    # 安装基础依赖
    && apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libmemcached-dev \
        zlib1g-dev \
        libcurl4-openssl-dev \
        libxml2-dev \
        libbz2-dev \
        libgmp-dev \
        libc-client-dev \
        libkrb5-dev \
        libicu-dev \
        libldap2-dev \
        libtidy-dev \
        libxslt1-dev \
        libzip-dev \
        libmagickwand-dev \
        --no-install-recommends && rm -rf /var/lib/apt/lists/* \

    # 开启php相关扩展
    && docker-php-ext-install -j$(nproc) \
        bcmath \
        bz2 \
        calendar \
        exif \
        gettext \
        gmp \
        intl \
        mysqli \
        pdo_mysql \
        soap \
        sockets \
        tidy \
        wddx \
        xmlrpc \
        xsl \
        zip \

    # 部分扩展需要配置参数后开启
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install -j$(nproc) imap \
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu \
    && docker-php-ext-install -j$(nproc) ldap \
    && docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install -j$(nproc) opcache \

    # pecl 安装其他扩展
    && pecl install redis-4.0.1 \
    && pecl install memcached \
    && pecl install xdebug \
    && pecl install mcrypt-1.0.1 \
    && pecl install imagick-3.4.3 \
    && docker-php-ext-enable redis memcached xdebug mcrypt imagick \

    && docker-php-source delete