FROM drupal:7

ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /usr/local/src/composer
ENV COMPOSER_BIN_DIR /usr/local/bin
ENV COMPOSER_VERSION 2.4.1

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    mariadb-client \
    && rm -rf /var/lib/apt/lists/*

RUN set -eux && \
  mkdir -p "${COMPOSER_HOME}" && \
  curl \
    --silent \
    --fail \
    --location \
    --retry 3 \
    --output /tmp/installer.php \
    --url https://raw.githubusercontent.com/composer/getcomposer.org/f24b8f860b95b52167f91bbd3e3a7bcafe043038/web/installer \
  && \
  echo 3137ad86bd990524ba1dedc2038309dfa6b63790d3ca52c28afea65dcc2eaead16fb33e9a72fd2a7a8240afaf26e065939a2d472f3b0eeaa575d1e8648f9bf19 /tmp/installer.php | sha512sum --strict --check && \
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

RUN yes | pecl install xdebug

RUN cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini
COPY xdebug.ini /usr/local/etc/php/conf.d
