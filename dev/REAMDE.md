Try:
```
docker run --name dpdev -p 8080:80 -d dravenk/dp:dev
````
OR:
Create a drupal 9 project with composer.
```
composer create-project drupal/recommended-project d9web "^9.0" -vvv
cd d9web
```
Run docker container with local project.
```
docker run --name some-drupal -p 8080:80 -v $PWD:/var/www/html -d dravenk/dp:dev
```

running phpunit test
```
docker rm -f dpdev
docker run --name dpdev -p 8080:80 -d dravenk/dp:dev
docker exec -it dpdev phpunit -c web/core/phpunit.xml
```