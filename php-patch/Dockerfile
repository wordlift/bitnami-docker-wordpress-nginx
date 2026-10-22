# Replaces PHP inside an existing wordlift/wordpress-nginx image with PHP 8.2.35.
# Everything else (WordPress, nginx, config, PECL extensions) stays as it is.
ARG BASE_IMAGE

# Compile PHP inside a copy of the original image, so it links against the same libraries.
FROM ${BASE_IMAGE} AS build
USER root

RUN apt-get update && apt-get install -y --no-install-recommends \
      build-essential autoconf pkg-config bison re2c xz-utils curl ca-certificates \
      libxml2-dev libssl-dev libbz2-dev libcurl4-openssl-dev libpng-dev libjpeg-dev \
      libwebp-dev libfreetype-dev libgmp-dev libicu-dev libonig-dev libreadline-dev \
      libsodium-dev libsqlite3-dev libxslt1-dev libzip-dev libtidy-dev libldap2-dev \
      libpq-dev freetds-dev

WORKDIR /build
RUN curl -fsSLo php.tar.xz https://www.php.net/distributions/php-8.2.35.tar.xz \
 && echo "5737b98601fe5ab1b8c5db0c82ed3158acb9283ed3081ffe2faffb9ac81455e6  php.tar.xz" | sha256sum -c - \
 && tar xJf php.tar.xz

# Bitnami's configure options, taken from `php -i` of the production images.
RUN cd php-8.2.35 && ./configure \
      --prefix=/opt/bitnami/php --with-config-file-scan-dir=/opt/bitnami/php/etc/conf.d \
      --with-libdir=lib/x86_64-linux-gnu --disable-rpath --enable-inline-optimization \
      --enable-fpm --with-fpm-user=daemon --with-fpm-group=daemon --enable-cgi \
      --with-zlib --with-zlib-dir --with-bz2 --with-openssl --with-curl --with-readline --with-sodium \
      --enable-soap --enable-sockets --enable-pcntl --enable-exif --enable-bcmath --enable-ftp \
      --enable-calendar --enable-mbstring --enable-ctype --enable-session --enable-intl \
      --enable-simplexml --with-iconv --with-gettext --with-gmp --with-xsl --with-tidy --with-pear \
      --with-libxml-dir=/usr --with-ldap=/usr/ \
      --enable-mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd \
      --with-pdo_sqlite --with-sqlite3 \
      --with-pgsql=shared --with-pdo-pgsql=shared --with-pdo-dblib=shared \
      --enable-gd --with-freetype --with-jpeg --with-webp --with-png-dir=/usr --with-zip \
      PKG_CONFIG_PATH=/opt/bitnami/common/lib/pkgconfig \
 && make -j"$(nproc)" \
 && make install \
 && mv /opt/bitnami/php/lib/php/extensions/no-debug-non-zts-*/*.so /opt/bitnami/php/lib/php/extensions/

# The original image with PHP's files swapped in.
FROM ${BASE_IMAGE}
COPY --from=build /opt/bitnami/php/bin     /opt/bitnami/php/bin
COPY --from=build /opt/bitnami/php/sbin    /opt/bitnami/php/sbin
COPY --from=build /opt/bitnami/php/lib     /opt/bitnami/php/lib
COPY --from=build /opt/bitnami/php/include /opt/bitnami/php/include
