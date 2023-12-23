FROM drupal:10-fpm

# set timezone
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update -y \
    && apt-get install -y apt-utils git ssh vim wget curl mariadb-client nginx unzip \
    && apt-get clean \
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
    rm -rf /var/lib/apt/lists/* \
    ## Adding bcmath for contrib module like drupal/commerce
    && docker-php-ext-install bcmath; \
    cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini

    ## Install composer. Overwrite older versions.
RUN curl -sS https://getcomposer.org/installer | \
    php -- --install-dir=/usr/local/bin/ --filename=composer

RUN composer require drush/drush
# ## Install drush-launcher
# RUN composer require drush/drush && \
#     wget -O /usr/local/bin/drush https://github.com/drush-ops/drush-launcher/releases/latest/download/drush.phar && \
#     chmod +x /usr/local/bin/drush

# Install apcu and uploadprogress
RUN pecl install apcu uploadprogress; docker-php-ext-enable apcu uploadprogress

COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./conf.d/default.conf /etc/nginx/conf.d/
COPY ./php/zz-docker.conf /usr/local/etc/php-fpm.d/zz-docker.conf
COPY ./php/conf.d/php-upload.ini /usr/local/etc/php/conf.d/php-uploads.ini
COPY ./php/conf.d/docker-php-ext-apcu.ini /usr/local/etc/php/conf.d/docker-php-ext-apcu.ini
COPY ./php/conf.d/docker-php-ext-uploadprogress.ini /usr/local/etc/php/conf.d/docker-php-ext-uploadprogress.ini

COPY entrypoint.sh /etc/entrypoint.sh
ENTRYPOINT ["sh", "/etc/entrypoint.sh"]
