# README

docker hub page: [https://hub.docker.com/repository/docker/yao3060/wordpress-php/general](https://hub.docker.com/repository/docker/yao3060/wordpress/general)

## php8.3 + frankenphp

```bash
docker build --tag yao3060/wordpress:php8.3-frankenphp  ./8.3-frankenphp
docker push yao3060/wordpress:php8.3-frankenphp
```

## php8.3 + apache

```bash
docker build --tag yao3060/wordpress:php8.3-apache  ./8.3-apache
docker push yao3060/wordpress:php8.3-apache
```

## php8.2 + apache

```bash
docker build --tag yao3060/wordpress:php8.2-apache  ./8.2-apache
docker push yao3060/wordpress:php8.2-apache
```

## php8.1 + apache

```bash
docker build --tag yao3060/wordpress:php8.1-apache ./8.1-apache
docker push yao3060/wordpress:php8.1-apache
```
