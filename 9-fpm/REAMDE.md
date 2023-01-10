Build
```
docker build . -t dravenk/dp:9-fpm
```
Test
```
docker run --name dpfpm -p 8877:80 -d dravenk/dp:9-fpm
```