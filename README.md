# dp

[![Build Status](
https://img.shields.io/docker/cloud/build/dravenk/dp.svg
)](
https://cloud.docker.com/repository/docker/dravenk/dp/builds
)

[![dockeri.co](https://dockeri.co/image/dravenk/dp)](https://hub.docker.com/r/dravenk/dp/tags)

Docker files for drupal

### build and push to upstream

```console
docker build -t dravenk/dp:10-fpm -t dravenk/dp:fpm -t dravenk/dp:latest 10-fpm
docker build -t dravenk/dp:10-dev -t dravenk/dp:dev 10-dev

docker build -t dravenk/dp:9-fpm 9-fpm
docker build -t dravenk/dp:9.4-php8.0-fpm 9.4-php8.0-fpm

docker build -t dravenk/dp:10-fpm -t dravenk/dp:fpm -t dravenk/dp:latest --no-cache 10-fpm

docker buildx build --platform linux/amd64,linux/arm64,linux/arm/v7 -t dravenk/dp:10-fpm --push 10-fpm
docker buildx build --platform linux/amd64,linux/arm64,linux/arm/v7 -t dravenk/dp:dev --push 10-dev

docker push -a dravenk/dp
```

running phpunit test
```
docker rm -f dpdev; docker run --name dpdev -d dravenk/dp:dev
docker exec -i dpdev sh -c 'su www-data'
phpunit -c web/core/phpunit.xml
```
