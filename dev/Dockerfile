FROM dravenk/dp

## https://www.drupal.org/docs/develop/using-composer/starting-a-site-using-drupal-composer-project-templates#s-adding-drupalcore-dev
# composer require drush/drush && \
# composer require --dev -W drupal/core-dev && \
RUN set -eux; composer require --dev -W drupal/core-dev:* -vvv
RUN set -eux; \
        composer global require drupal/coder && \
        composer global require dealerdirect/phpcodesniffer-composer-installer

## https://www.drupal.org/docs/develop/using-composer/starting-a-site-using-drupal-composer-project-templates#s-workaround
RUN chmod u+w web/sites/default

RUN { \
    echo 'export PATH="$PATH:$HOME/.composer/vendor/bin"'; \
    echo 'alias drupalcs="phpcs --standard=Drupal --extensions='php,module,inc,install,test,profile,theme,css,info,txt,md,yml'"'; \
    echo 'alias drupalcsp="phpcs --standard=DrupalPractice --extensions='php,module,inc,install,test,profile,theme,css,info,txt,md,yml'"'; \
    echo 'alias drupalcbf="phpcbf --standard=Drupal --extensions='php,module,inc,install,test,profile,theme,css,info,txt,md,yml'"'; \
    } >> /root/.bashrc

RUN /bin/bash -c "source /root/.bashrc"

# See https://xdebug.org/docs/remote
RUN { \
    echo '[xdebug]'; \
    echo 'xdebug.mode=develop,coverage,debug'; \
    echo 'xdebug.start_with_request=yes'; \
    echo 'xdebug.discover_client_host=true'; \
    echo 'xdebug.client_host=host.docker.internal'; \
    } >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
# Install xdebug
RUN pecl install xdebug; docker-php-ext-enable xdebug