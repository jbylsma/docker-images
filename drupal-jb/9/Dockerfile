FROM drupal:9

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      git \
      mariadb-client \
    && rm -rf /var/lib/apt/lists/*

RUN composer global require drush/drush

RUN yes | pecl install apcu xdebug
RUN echo 'extension = apcu.so' >/usr/local/etc/php/conf.d/apcu.ini

RUN cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini
RUN sed 's#memory_limit = 128M#memory_limit = 1G#' /usr/local/etc/php/php.ini-production >/usr/local/etc/php/php.ini
COPY xdebug.ini /usr/local/etc/php/conf.d
