FROM php:8.1-fpm-alpine

# Packages required for build but not runtime.
ENV MY_BUILD_DEPS \
        autoconf \
        musl-dev \
        libc-dev \
        gcc \
        g++ \
        dpkg \
        dpkg-dev \
        pkgconf \
        re2c

# PhpBB distribution.
ENV MY_FORUM_DIST \
        https://download.phpbb.com/pub/release/3.3/3.3.8/phpBB-3.3.8.tar.bz2

# Output name for the archive above.
ENV MY_FORUM_DIST_OUT \
        dist.tar.bz2

RUN apk add --no-cache \
        bash \
        curl \
        $MY_BUILD_DEPS \
        libpng-dev \
        libpq-dev
RUN docker-php-ext-install \
        gd \
        pgsql
RUN apk del --purge \
        $MY_BUILD_DEPS

COPY ./php.ini /etc/local/php/

SHELL ["/bin/bash", "-c"]
WORKDIR /var/www/

RUN rm -r /var/www/html/

RUN curl -o $MY_FORUM_DIST_OUT $MY_FORUM_DIST \
    && tar -xvf $MY_FORUM_DIST_OUT \
    && rm $MY_FORUM_DIST_OUT

RUN mv ./phpBB3 ./html

RUN chown -R www-data:www-data ./html \
    && chmod 666 ./html/config.php \
    && chmod -R 777 \
        ./html/store/ \
        ./html/cache/ \
        ./html/files/ \
        ./html/images/avatars/upload/

USER www-data

CMD php-fpm
EXPOSE 9000
