Build
```
docker build . -t dravenk/drupal:fpm
```
Test
```
docker run --name dpfpm -p 8877:80 -d dravenk/drupal:fpm
```