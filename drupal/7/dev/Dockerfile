FROM dravenk/drupal:7

# Install xdebug
RUN pecl install xdebug && docker-php-ext-enable xdebug
# See https://xdebug.org/docs/remote
RUN { \
    echo '[xdebug]'; \
    echo 'xdebug.mode=debug,develop,coverage'; \
    echo 'xdebug.start_with_request=yes'; \
    echo 'xdebug.discover_client_host=0'; \
    echo 'xdebug.client_host=host.docker.internal'; \
    } >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
