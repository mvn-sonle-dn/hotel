FROM phusion/baseimage:bionic-1.0.0

MAINTAINER Mahmoud Zalt <mahmoud@zalt.me>

RUN DEBIAN_FRONTEND=noninteractive
RUN locale-gen en_US.UTF-8

ENV LANGUAGE=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8
ENV LC_CTYPE=UTF-8
ENV LANG=en_US.UTF-8
ENV TERM xterm

# Install "software-properties-common" (for the "add-apt-repository")
RUN apt-get update && apt-get install -y \
    software-properties-common php-pear

RUN apt-get install -y software-properties-common && \
    add-apt-repository -y ppa:ondrej/php

# Install PHP-CLI 7.2, some php extentions and some useful tools with apt
RUN apt-get update && \
    apt-get install -y --allow-downgrades --allow-remove-essential \
        --allow-change-held-packages \
        php7.2-cli \
        php7.2-common \
        php7.2-curl \
        php7.2-intl \
        php7.2-json \
        php7.2-xml \
        php7.2-mbstring \
        php7.2-mysql \
        php7.2-zip \
        php7.2-bcmath \
        php7.2-memcached \
        php7.2-gd \
        php7.2-dev \
        pkg-config \
        libcurl4-openssl-dev \
        libedit-dev \
        libssl-dev \
        libxml2-dev \
        xz-utils \
        git \
        curl \
        vim \
        mysql-client-5.7 \
    && apt-get clean

# Install node 8
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash -
RUN apt-get install -y nodejs

# Install apidocjs
RUN update-alternatives --install /usr/bin/node node /usr/bin/nodejs 10
VOLUME ["/var/www/laravel"]
# RUN cd /var/www/laravel && npm install npm install apidoc -g

# Install composer
RUN curl -s http://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer

# Source the bash
ENV PATH /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/var/www/laravel/vendor/bin

# Install xdebug
RUN apt-get update \
    && apt-get -y --no-install-recommends install  php-xdebug

# Clean up, to free some space
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ADD cronjobs /etc/cron.d/cronjobs
RUN /usr/bin/crontab /etc/cron.d/cronjobs
ENTRYPOINT ["cron", "-f", "/etc/cron.d/cronjobs"]

WORKDIR /var/www/laravel
