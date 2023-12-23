# drupal

### build and push to [docker hub](https://hub.docker.com/r/dravenk/drupal)
```console
docker buildx build --platform linux/amd64,linux/arm64,linux/arm/v7 -t dravenk/drupal:latest drupal/10/fpm --push
docker buildx build --platform linux/amd64,linux/arm64,linux/arm/v7 -t dravenk/drupal:dev drupal/10/dev --push

docker buildx build --platform linux/amd64,linux/arm64,linux/arm/v7 -t dravenk/drupal:9-fpm drupal/9/fpm --push
docker buildx build --platform linux/amd64,linux/arm64,linux/arm/v7 -t dravenk/drupal:9.4-php8.0-fpm drupal/9/php8.0 --push
```

#### Running phpunit test
```
docker rm -f drupaldev; docker run --name drupaldev -d dravenk/drupal:dev
docker exec -i drupaldev sh -c 'su www-data'
phpunit -c web/core/phpunit.xml web/core/modules/action
```
