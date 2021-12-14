FROM drupal:fpm

# set timezone
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update -y \
    && apt-get install -y git ssh vim wget mariadb-client nginx \
    && apt-get clean \
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
    rm -rf /var/lib/apt/lists/* \
    ## Adding bcmath for contrib module like drupal/commerce
    && docker-php-ext-install bcmath; \
    ## Copy php.ini
    cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini

ENV DRUSH_VERSION 8.4.6
RUN  \
    ## Install composer. Overwrite older versions.
    curl -sS https://getcomposer.org/installer | \
    php -- --install-dir=/usr/local/bin/ --filename=composer \
    ## Install drush
    && curl -L --silent https://github.com/drush-ops/drush/releases/download/${DRUSH_VERSION}/drush.phar \
    > /usr/local/bin/drush8 && chmod +x /usr/local/bin/drush8; \
    ## Install drush-launcher
    wget -O /usr/local/bin/drush https://github.com/drush-ops/drush-launcher/releases/latest/download/drush.phar \
    && chmod +x /usr/local/bin/drush; 
    # Install drupal-console
    # wget -O drupal.phar https://drupalconsole.com/installer \
    # && mv drupal.phar /usr/local/bin/drupal \
    # && chmod +x /usr/local/bin/drupal

# Install apcu and uploadprogress
RUN pecl install apcu uploadprogress && docker-php-ext-enable apcu uploadprogress

EXPOSE 80
EXPOSE 443

COPY ./conf.d/site.conf /etc/nginx/conf.d/
COPY ./php/zz-docker.conf /usr/local/etc/php-fpm.d/zz-docker.conf
COPY ./php/conf.d/php-upload.ini /usr/local/etc/php/conf.d/php-uploads.ini
COPY ./php/conf.d/docker-php-ext-apcu.ini /usr/local/etc/php/conf.d/docker-php-ext-apcu.ini
COPY ./php/conf.d/docker-php-ext-uploadprogress.ini /usr/local/etc/php/conf.d/docker-php-ext-uploadprogress.ini

COPY entrypoint.sh /etc/entrypoint.sh

ENTRYPOINT ["sh", "/etc/entrypoint.sh"]
