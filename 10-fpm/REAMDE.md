Build
```
docker build . -t dravenk/dp:fpm
```
Test
```
docker run --name dpfpm -p 8877:80 -d dravenk/dp:fpm
```