FROM dravenk/dp:fpm

RUN set -eux; \
        composer global require drupal/coder && \
        composer global require dealerdirect/phpcodesniffer-composer-installer

RUN { \
    echo 'export PATH="$PATH:$HOME/.composer/vendor/bin"'; \
    echo 'alias drupalcs="phpcs --standard=Drupal --extensions='php,module,inc,install,test,profile,theme,css,info,txt,md,yml'"'; \
    echo 'alias drupalcsp="phpcs --standard=DrupalPractice --extensions='php,module,inc,install,test,profile,theme,css,info,txt,md,yml'"'; \
    echo 'alias drupalcbf="phpcbf --standard=Drupal --extensions='php,module,inc,install,test,profile,theme,css,info,txt,md,yml'"'; \
    } >> /root/.bashrc

# Install xdebug
RUN pecl install xdebug && docker-php-ext-enable xdebug

# See https://xdebug.org/docs/remote
RUN { \
    echo '[xdebug]'; \
    echo 'xdebug.mode=develop,coverage,debug'; \
    echo 'xdebug.start_with_request=yes'; \
    echo 'xdebug.discover_client_host=true'; \
    echo 'xdebug.client_host=host.docker.internal'; \
    } >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
