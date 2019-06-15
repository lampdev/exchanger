FROM debian:9.3

ARG SYSTEM_TZ

ENV _RH_HTTPD=/etc/apache2 \
    _RH_PHP=/php/7.2
ENV _RH_PHP_D=/etc${_RH_PHP}

RUN ln -snf /usr/share/zoneinfo/$SYSTEM_TZ /etc/localtime && echo $SYSTEM_TZ > /etc/timezone

RUN set -xe \
    && export DEBIAN_FRONTEND=noninteractive \
    && apt-get update \
    && apt-get -y install \
        apt-utils \
        apache2 \
        apt-transport-https \
        lsb-release \
        ca-certificates \
        curl \
        zip \
        unzip \
        alien \
        libgcj-common \
        ghostscript \
        git \
        wget \
        fontconfig \
        fonts-liberation \
        xdg-utils \
        libasound2 \
        libatk1.0-0 \
        libcairo2 \
        libgtk-3-0 \
        libpango-1.0-0 \
        libappindicator3-1 \
        libxss1

RUN set -xe \
    && wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg \
    && echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/php.list \
    && curl -o- -L https://deb.nodesource.com/setup_8.x | bash \
    && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN set -xe \
    && apt-get update \
    && apt-get -y install \
        php7.2 \
        php7.2-dev \
        php7.2-bcmath \
        php7.2-common \
        php7.2-curl \
        php7.2-dom \
        php7.2-gd \
        php7.2-mbstring \
        php7.2-mysqli \
        php7.2-mysqlnd \
        php7.2-pdo \
        php7.2-tidy \
        php7.2-xml \
        php7.2-xdebug \
        php7.2-zip \
        php7.2-intl \
        libapache2-mod-php7.2 \
        php-pear \
        pdftk \
        nodejs \
        yarn \
        nano \
        cron \
    && apt-get -y upgrade

RUN set -xe \
    && curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer && chmod +x /usr/local/bin/composer

RUN set -xe \
    && update-alternatives --set php /usr/bin/php7.2

RUN set -xe \
    && rm ${_RH_PHP_D}/apache2/conf.d/20-xdebug.ini \
    && rm ${_RH_PHP_D}/cli/conf.d/20-xdebug.ini

ENV \
    ETC_HTTPD=/etc/apache2 \
    DOCUMENT_ROOT=/var/www/html \
    PHP_PATH=/usr/bin/php

RUN /bin/echo -e "#!/bin/bash\n\
apache2 -v\n\
exec apachectl -D FOREGROUND\n\
" > /usr/sbin/docker-entrypoint.sh \
    && chmod +x /usr/sbin/docker-entrypoint.sh

CMD /usr/sbin/docker-entrypoint.sh
