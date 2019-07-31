#
# docker build -t spafftank/phocker:latest -f ./workspace/Dockerfile ./workspace
#

FROM ubuntu:18.04

ENV DEBIAN_FRONTEND=noninteractive \
    LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8 \
    COMPOSER_ALLOW_SUPERUSER=1 \
    FPM_CONF=/etc/php/7.2/fpm/pool.d/www.conf \
    PHP_VERSION=7.2

RUN apt-get update && \
    apt-get install nano && \
    apt-get install -y language-pack-en-base software-properties-common && \
    add-apt-repository ppa:ondrej/php && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN apt-get update && \
    apt-get install -y \
        git zip curl \
        autoconf build-essential libpng-dev nasm libtool pkg-config \
        php7.2 \
        php7.2-cli \
        php7.2-common \
        php7.2-curl \
        php7.2-gd \
        php7.2-json \
        php7.2-mysql \
        php7.2-xml \
        php7.2-bcmath \
        php7.2-bz2 \
        php7.2-fpm \
        php7.2-imap \
        php7.2-mbstring \
        php7.2-pdo \
        php7.2-zip \
        php-redis \
        php-imagick \
        php-xdebug \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN composer global require hirak/prestissimo

RUN echo 'Europe/London' > /etc/timezone && dpkg-reconfigure tzdata

RUN sed -e '/;daemonize/s/^;//' \
        -e 's/daemonize = yes/daemonize = no/' \
        -e '/error_log/s/=.*/= \/proc\/self\/fd\/2/' \
        -i /etc/php/7.2/fpm/php-fpm.conf && \
    sed -e '/catch_workers_output/s/^;//' \
        -e '/clear_env/s/^;//' \
        -e '/pm =/s/=.*/= ondemand/' \
        -e '/pm.max_children/s/5$/25/' \
        -e '/pm.max_requests/s/^;//' \
        -e '/pm.max_requests/s/500$/200/' \
        -e '/listen =/s/=.*/= 0.0.0.0:9000/' \
        -i "${FPM_CONF}"

WORKDIR /var/www

RUN usermod -u 1000 www-data
RUN mkdir "/run/php"

EXPOSE 9000

CMD ["/usr/sbin/php-fpm7.2", "--allow-to-run-as-root", "-c /etc/php/7.2/fpm", "--nodaemonize", "php -S localhost:9000"]
