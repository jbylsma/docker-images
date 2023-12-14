FROM drupal:7

ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /usr/local/src/composer
ENV COMPOSER_BIN_DIR /usr/local/bin
ENV COMPOSER_VERSION 2.5.8

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    mariadb-client sqlite3 \
    && rm -rf /var/lib/apt/lists/*

RUN set -eux && \
  mkdir -p "${COMPOSER_HOME}" && \
  curl \
    --silent \
    --fail \
    --location \
    --retry 3 \
    --output /tmp/installer.php \
    --url https://raw.githubusercontent.com/composer/getcomposer.org/6674c87670ba2e2b2df959e236a1a6d229672d6c/web/installer \
  && \
  echo 55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae /tmp/installer.php | sha384sum --strict --check && \
  php /tmp/installer.php \
    --no-ansi \
    --install-dir=/usr/local/bin \
    --filename=composer \
    --version=${COMPOSER_VERSION} \
  && \
  composer --ansi --version --no-interaction && \
  composer diagnose && \
  rm -f /tmp/installer.php && \
  find /tmp -type d -exec chmod -v 1777 {} +

RUN composer global require drush/drush:^8

RUN yes | pecl install xdebug-3.1.5

RUN cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini && \
    echo 'error_log = STDERR' >>/usr/local/etc/php/php.ini
COPY xdebug.ini /usr/local/etc/php/conf.d
