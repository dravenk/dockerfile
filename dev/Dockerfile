FROM dravenk/dp

## https://www.drupal.org/docs/develop/using-composer/starting-a-site-using-drupal-composer-project-templates#s-adding-drupalcore-dev
RUN set -eux; composer require --dev -W drupal/core-dev:* -vvv
RUN set -eux; \
        # composer global require drupal/coder && \
        # composer global require dealerdirect/phpcodesniffer-composer-installer
        composer require drupal/coder && \
        composer require dealerdirect/phpcodesniffer-composer-installer

RUN { \
    echo 'export PATH="$PATH:$HOME/.composer/vendor/bin"'; \
    echo 'alias drupalcs="phpcs --standard=Drupal --extensions='php,module,inc,install,test,profile,theme,css,info,txt,md,yml'"'; \
    echo 'alias drupalcsp="phpcs --standard=DrupalPractice --extensions='php,module,inc,install,test,profile,theme,css,info,txt,md,yml'"'; \
    echo 'alias drupalcbf="phpcbf --standard=Drupal --extensions='php,module,inc,install,test,profile,theme,css,info,txt,md,yml'"'; \
    } >> /root/.bashrc

RUN /bin/bash -c "source /root/.bashrc"

# Install xdebug
# See https://xdebug.org/docs/remote
RUN { \
    echo '[xdebug]'; \
    # echo 'xdebug.mode=off'; \
    echo 'xdebug.mode=develop,coverage,debug'; \
    echo 'xdebug.start_with_request=yes'; \
    echo 'xdebug.discover_client_host=true'; \
    echo 'xdebug.client_host=host.docker.internal'; \
    } >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && pecl install xdebug; docker-php-ext-enable xdebug
# RUN pecl install xdebug; docker-php-ext-enable xdebug

ENV DEFAULT_DIR /opt/drupal/web/sites/default
ENV FILES_DIR /opt/drupal/web/sites/default/files
ENV WEB_DIR /opt/drupal/web
ENV BROWSERTEST_OUTPUT_DIRECTORY ${FILES_DIR}/simpletest/browser_output
ENV PHPUNIT_CONF ${WEB_DIR}/core/phpunit.xml
ENV PHPCS_CONF ${WEB_DIR}/core/phpcs.xml

## https://www.drupal.org/docs/develop/using-composer/starting-a-site-using-drupal-composer-project-templates#s-workaround
# RUN chmod u+w web/sites/default \
RUN mkdir ${FILES_DIR} \
    && chmod -R 777 ${FILES_DIR} \
    && cp ${DEFAULT_DIR}/default.settings.php ${DEFAULT_DIR}/settings.php \
    && chmod 777 ${DEFAULT_DIR}/settings.php

RUN rm -rf vendor/drupal/coder/.git

RUN composer require --dev phpspec/prophecy-phpunit:^2 \
    && composer require cweagans/composer-patches \
    && composer up

RUN composer config minimum-stability dev \
    && composer config allow-plugins.composer/installers true \
    && composer config allow-plugins.dealerdirect/phpcodesniffer-composer-installer true \
    && composer config allow-plugins.drupal/core-composer-scaffold true \
    && composer config allow-plugins.drupal/core-project-message true \
    && composer config allow-plugins.cweagans/composer-patches true \
    && composer config allow-plugins.drupal/console-extend-plugin true \
    && composer config allow-plugins.wikimedia/composer-merge-plugin true

RUN wget https://raw.githubusercontent.com/drupal-composer/drupal-project/9.x/.gitignore -O .gitignore \
    && git config --global user.email "you@example.com" && git config --global user.name "Your Name" \
    && git init && git add . && git commit -m 'Init'

# https://www.drupal.org/docs/automated-testing/phpunit-in-drupal/running-phpunit-tests#s-configure-phpunit

RUN mkdir -p ${BROWSERTEST_OUTPUT_DIRECTORY} \
    && chown -R www-data:www-data ${WEB_DIR} \
    # && chmod -R g+rw ${FILES_DIR}
    && chmod -R 777 ${FILES_DIR}

#Install using SQLite in-memory database.  --account-pass=admin
# RUN drush si -y --db-url=sqlite:sites/default/files/.ht.sqlite#test
RUN cp ${WEB_DIR}/core/phpunit.xml.dist ${PHPUNIT_CONF} && \
    phpunit -c ${PHPUNIT_CONF} --migrate-configuration && \
    cp ${WEB_DIR}/core/phpcs.xml.dist ${PHPCS_CONF}

# RUN mkdir -p /var/www/html/sites/simpletest/browser_output && chmod -Rv 777 /var/www/html/sites/simpletest/browser_output

RUN sed -i 's|name="SIMPLETEST_BASE_URL" value=""|name="SIMPLETEST_BASE_URL" value="http://localhost"|g' ${PHPUNIT_CONF}
RUN sed -i 's|name="SIMPLETEST_DB" value=""|name="SIMPLETEST_DB" value="sqlite://sites/default/files/.ht.sqlite"|g' ${PHPUNIT_CONF}
# RUN sed -i 's|name="SIMPLETEST_DB" value=""|name="SIMPLETEST_DB" value="sqlite://:memory:"|g' web/core/phpunit.xml
# RUN sed -i 's|name="BROWSERTEST_OUTPUT_DIRECTORY" value=""|name="BROWSERTEST_OUTPUT_DIRECTORY" value="/opt/drupal/web/sites/default/files/simpletest/browser_output"|g' ${PHPUNIT_CONF}
RUN sed -i 's|name="BROWSERTEST_OUTPUT_DIRECTORY" value=""|name="BROWSERTEST_OUTPUT_DIRECTORY" value="${BROWSERTEST_OUTPUT_DIRECTORY}"|g' ${PHPUNIT_CONF}
RUN sed -i 's|name="<!-- <env name="SYMFONY_DEPRECATIONS_HELPER" value="disabled"/> -->|name="<env name="SYMFONY_DEPRECATIONS_HELPER" value="disabled"/>|g' ${PHPUNIT_CONF}

# running phpunit test
# phpunit -c /opt/drupal/web/core/phpunit.xml
# EXPOSE 9003

#https://github.com/docker-library/php/blob/6e600f59a4405b5066eb78f5f02180212dffd065/7.0/fpm/php-fpm.conf
# COPY ./php/conf.d/docker-php-ext-log.ini /usr/local/etc/php/conf.d/docker-php-ext-log.ini