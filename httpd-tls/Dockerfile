FROM httpd:2.4

RUN yes '' |\
        openssl req -new -x509 -days 365 -nodes -out /etc/ssl/certs/ssl-cert-snakeoil.pem -keyout /etc/ssl/private/ssl-cert-snakeoil.key && \
        sed -i \
          -e 's/^#\(Include .*httpd-ssl.conf\)/\1/' \
          -e 's/^#\(LoadModule .*mod_ssl.so\)/\1/' \
          -e 's/^#\(LoadModule .*mod_socache_shmcb.so\)/\1/' \
          conf/httpd.conf && \
        sed -i \
          -e 's#/usr/local/apache2/conf/server.crt#/etc/ssl/certs/ssl-cert-snakeoil.pem#' \
          -e 's#/usr/local/apache2/conf/server.key#/etc/ssl/private/ssl-cert-snakeoil.key#' \
          conf/extra/httpd-ssl.conf
