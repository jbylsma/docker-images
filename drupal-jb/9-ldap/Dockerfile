FROM drupal-jb:9

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      libldap-dev \
    && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install ldap
