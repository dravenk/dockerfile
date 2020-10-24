FROM dravenk/dp:fpm

RUN rm -rf /var/www/html \
  mkdir /var/www/html

WORKDIR /var/www/html

RUN composer create-project drupal/recommended-project:^9.0 . -vvv; \
  chown -R www-data:www-data web/sites web/modules web/themes

COPY ./conf.d/site.conf /etc/nginx/conf.d/
