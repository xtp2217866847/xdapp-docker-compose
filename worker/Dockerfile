#
#--------------------------------------------------------------------------
# Image Setup
#--------------------------------------------------------------------------
#

FROM ubuntu:latest

# Install "PHP Extentions", "libraries", "Software's"
RUN \
    # Change application source from deb.debian.org to aliyun source
    sed -i 's/deb.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list && \
    sed -i 's/security.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list && \
    sed -i 's/security-cdn.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list

RUN  sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list && \
      apt-get clean && \
      apt-get update

USER root

RUN  \
    apt -y install software-properties-common && \
    add-apt-repository ppa:ondrej/php && \
    apt-get update && \
    apt -y install \
        php7.4 \
        php7.4-cli \
        php7.4-common \
        php7.4-curl \
        php7.4-intl \
        php7.4-json \
        php7.4-xml \
        php7.4-mbstring \
        php7.4-mysql \
        php7.4-pgsql \
#        php7.4-sqlite \
#        php7.4-sqlite3 \
        php7.4-zip \
        php7.4-bcmath \
        php7.4-memcached \
        php7.4-gd \
        php7.4-dev \
#        pkg-config \
#        libcurl4-openssl-dev \
#        libedit-dev \
#        libssl-dev \
#        libxml2-dev \
#        xz-utils \
#        libsqlite3-dev \
#        rsync \
#        sqlite3 \
        git \
        curl \
        vim \
        nano


RUN set -eux; \
        \
        php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" && \
        php composer-setup.php && \
        php -r "unlink('composer-setup.php');" && \
        mv composer.phar /usr/local/bin/composer && \
        composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

RUN \
    apt-get install php-dev php-pear autoconf automake libtool -y && \
    pecl install swoole && \
    echo "extension=swoole.so" >> /etc/php/7.4/cli/php.ini

WORKDIR /var/www
#CMD ["worker"]