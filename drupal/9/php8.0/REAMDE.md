Build
```
docker build . -t dravenk/drupal:9.4-php8.0-fpm
```
Test
```
docker run --name dpfpm -p 8877:80 -d dravenk/drupal:9.4-php8.0-fpm
```