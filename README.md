# README

docker hub page: https://hub.docker.com/repository/docker/yao3060/wordpress-php/general

## php8.2 + apache

```
docker build --tag yao3060/wordpress:php8.2-apache  -f ./Dockerfile.8.2-apache .
docker push yao3060/wordpress:php8.2-apache
```

## php8.1 + apache

```
docker build --tag yao3060/wordpress:php8.1-apache  -f ./Dockerfile.8.1-apache .
docker push yao3060/wordpress:php8.1-apache
```
