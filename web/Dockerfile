FROM dravenk/dp:fpm
COPY ./conf.d/site.conf /etc/nginx/conf.d/

WORKDIR /var/www/html

# Install drush-launcher
RUN mv  /usr/local/bin/drush  /usr/local/bin/drush8 && \
  wget -O /usr/local/bin/drush \ 
  https://github.com/drush-ops/drush-launcher/releases/latest/download/drush.phar \
  && chmod +x /usr/local/bin/drush

RUN rm -rf * && rm -rf .*; \
  composer create-project drupal/recommended-project:^9.0 . -vvv; \
  chown -R www-data:www-data web/sites web/modules web/themes
