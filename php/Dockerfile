FROM php:7.3-cli
RUN sed -ri "s/(httpredir|deb|security).debian.org/mirrors.aliyun.com/g" /etc/apt/sources.list
RUN apt-get update \
    && apt-get install -y libyaml-dev libssl-dev curl librdkafka-dev net-tools iputils-ping git

RUN docker-php-ext-install -j$(nproc) mysqli pdo pdo_mysql \
    && pecl install mongodb \
    && pecl install msgpack \
    && pecl install redis \
    && pecl install ds \

    && pecl install yaml \
    && pecl install rdkafka \
    && pecl install hprose \
    && docker-php-ext-enable msgpack redis ds yaml rdkafka hprose

RUN git clone --recursive --depth=1 https://github.com/kjdev/php-ext-snappy.git \
    && cd php-ext-snappy \
    && phpize && ./configure && make && make install \
    && docker-php-ext-enable snappy

RUN printf "no\nyes\n" | pecl install swoole \
    && docker-php-ext-enable swoole \
    && echo "swoole.use_shortname = 'Off'" >> /usr/local/etc/php/conf.d/docker-php-ext-swoole.ini

RUN apt-get install -y procps htop axel

#添加composer
RUN set -eux; \
        \
        php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" && \
        php composer-setup.php && \
        php -r "unlink('composer-setup.php');" && \
        mv composer.phar /usr/local/bin/composer && \
        composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

#添加sdebug调试
RUN git clone https://github.com/mabu233/sdebug.git && cd sdebug \
    && phpize \
    && ./configure \
    && make clean \
    && make \
    && make install
COPY ./docker-php-ext-sdebug.ini /usr/local/etc/php/conf.d

RUN docker-php-ext-install bcmath

#安装protobuf
RUN apt-get install -y wget autoconf automake libtool curl make g++ unzip
COPY ./protobuf-cpp-3.7.1.tar.gz /usr/local/protobuf-cpp-3.7.1.tar.gz
RUN  cd /usr/local && tar -xzvf protobuf-cpp-3.7.1.tar.gz && cd protobuf-3.7.1 \
    && ./autogen.sh && ./configure \
    && make && make check && make install && ldconfig && \


#COPY . /usr/src/xdapp-server
WORKDIR /usr/src/xdapp-server
#CMD [ "./bin/Server start" ]