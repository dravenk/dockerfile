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
docker build -t dravenk/dp:fpm fpm
docker tag dravenk/dp:fpm dravenk/dp:latest
docker build -t dravenk/dp:dev dev

docker push dravenk/dp:fpm
docker push dravenk/dp:latest
docker push dravenk/dp:dev
```

running phpunit test
```
docker rm -f dpdev; docker run --name dpdev -d dravenk/dp:dev
docker exec -it dpdev phpunit -c web/core/phpunit.xml
```