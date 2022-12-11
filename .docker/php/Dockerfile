FROM php:8.0-fpm-alpine

# Get arguments from docker-file
ARG UID
ARG GID

# Set environment variables
ENV UID=${UID}
ENV GID=${GID}

# Add group and user named developer
RUN addgroup -g ${GID} --system developer
RUN adduser -G developer --system -D -s /bin/sh -u ${UID} developer

# Configure PHP-FPM
#RUN /bin/cp /usr/local/etc/php/php.inielopment /usr/local/etc/php/php.ini
#COPY .docker/php/php.ini /usr/local/etc/php/conf.d/custom.ini
#COPY .docker/php/www.conf /usr/local/etc/php-fpm.d/www.conf

# Run PHP with user developer
#RUN sed -i "s/user = www-data/user = developer/g" /usr/local/etc/php-fpm.d/www.conf
#RUN sed -i "s/group = www-data/group = developer/g" /usr/local/etc/php-fpm.d/www.conf

# Setup document root
WORKDIR /var/www/html

RUN apk --update add \
  git \
  vim \
  wget \
  curl \
  build-base \
  libmcrypt-dev \
  libxml2-dev \
  libzip-dev \
  libxslt-dev \
  libgcrypt-dev \
  zlib-dev \
  gettext-dev \
  pcre-dev \
  autoconf \
  oniguruma-dev \
  postgresql-dev \
  openssl-dev \
  freetype-dev \
  libjpeg-turbo-dev \
  jpeg-dev \
  libpng-dev \
  imagemagick-dev \
  linux-headers &&\
  rm /var/cache/apk/*

RUN pecl channel-update pecl.php.net && \
    pecl install mcrypt redis xdebug && \
    rm -rf /tmp/pear

RUN docker-php-ext-enable redis xdebug

RUN docker-php-ext-install \
    mysqli \
    mbstring \
    pdo \
    pdo_mysql \
    xml \
    pcntl \
    bcmath \
    pdo_pgsql \
    zip \
    intl \
    gettext \
    soap \
    sockets \
    xsl \
    opcache

RUN docker-php-ext-configure gd --with-freetype=/usr/lib/ --with-jpeg=/usr/lib/ && \
    docker-php-ext-install gd

# Install composer form it's latest image
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Make sure files/folders needed by the processes are accessable when they run under the developer user
# RUN chown -R developer.developer /var/www/html /run /var/lib/nginx /var/log/nginx
RUN chown -R developer.developer /var/www/html /run

# Switch to use a non-root user from here on
USER developer

# Configure a healthcheck to validate that everything is up & running
#HEALTHCHECK --timeout=10s CMD curl --silent --fail http://localhost/fpm-ping

EXPOSE 9000
CMD ["php-fpm", "-y", "/usr/local/etc/php-fpm.conf", "-R"]
