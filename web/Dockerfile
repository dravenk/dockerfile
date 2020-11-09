FROM dravenk/dp:fpm

RUN composer create-project drupal/recommended-project . -vvv; \
  chown -R www-data:www-data web/sites web/modules web/themes; \
  cp web/example.gitignore web/.gitignore

COPY ./conf.d/site.conf /etc/nginx/conf.d/
