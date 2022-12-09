FROM drupal:fpm

# set timezone
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# RUN export DEBIAN_FRONTEND=noninteractive
RUN apt-get update -y \
    && apt-get install -y apt-utils git ssh vim wget curl mariadb-client nginx unzip \
    && apt-get clean \
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
    rm -rf /var/lib/apt/lists/* \
    ## Adding bcmath for contrib module like drupal/commerce
    && docker-php-ext-install bcmath; \
    ## Copy php.ini
    cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini

    ## Install composer. Overwrite older versions.
RUN curl -sS https://getcomposer.org/installer | \
    php -- --install-dir=/usr/local/bin/ --filename=composer

ENV DRUSH_VERSION 8.4.10
    ## Install drush 8
RUN wget -O /usr/local/bin/drush8 https://github.com/drush-ops/drush/releases/download/${DRUSH_VERSION}/drush.phar && \
    chmod +x /usr/local/bin/drush8

    ## Install drush-launcher
RUN composer require drush/drush && \
    wget -O /usr/local/bin/drush https://github.com/drush-ops/drush-launcher/releases/latest/download/drush.phar && \
    chmod +x /usr/local/bin/drush;\
    # Install drupal-console
    curl https://drupalconsole.com/installer -L -o drupal.phar && \
    mv drupal.phar /usr/local/bin/drupal && \
    chmod +x /usr/local/bin/drupal

# Install apcu and uploadprogress
RUN pecl install apcu uploadprogress; docker-php-ext-enable apcu uploadprogress

## Remove this temporary solution after the issue XXX has been fixed.
## See https://github.com/drush-ops/drush-launcher/issues/84
RUN echo "alias drush='./vendor/bin/drush'" >> ~/.bashrc && \
    /bin/bash -c "source ~/.bashrc"

COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./conf.d/default.conf /etc/nginx/conf.d/
# COPY ./php/docker.conf /usr/local/etc/php-fpm.d/docker.conf
# COPY ./php/php-fpm.conf /usr/local/etc/php-fpm.d/docker.conf
COPY ./php/zz-docker.conf /usr/local/etc/php-fpm.d/zz-docker.conf
COPY ./php/conf.d/php-upload.ini /usr/local/etc/php/conf.d/php-uploads.ini
COPY ./php/conf.d/docker-php-ext-apcu.ini /usr/local/etc/php/conf.d/docker-php-ext-apcu.ini
COPY ./php/conf.d/docker-php-ext-uploadprogress.ini /usr/local/etc/php/conf.d/docker-php-ext-uploadprogress.ini

# EXPOSE 80
# EXPOSE 443

COPY entrypoint.sh /etc/entrypoint.sh
ENTRYPOINT ["sh", "/etc/entrypoint.sh"]
