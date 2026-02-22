# README

- **WordPress 镜像**：Docker Hub [yao3060/wordpress](https://hub.docker.com/repository/docker/yao3060/wordpress/general)
- **Laravel 基础镜像**：Docker Hub [yao3060/laravel](https://hub.docker.com/r/yao3060/laravel)

基于官方 PHP 与 [FrankenPHP](https://frankenphp.dev/) 的 WordPress 镜像，预装 WordPress 所需 PHP 扩展（bcmath、exif、gd、intl、mysqli、zip、imagick、redis、opcache 等），支持多架构（amd64 / arm64）。另提供 Laravel Octane + FrankenPHP 基础镜像，供生产镜像基于该镜像复制应用文件后运行。

**Debian 发行版本**


| 版本号 | 代号       | 发布日期       | 支持截止日期     | LTS 支持截止日期 | ELTS 支持截止日期 |
| --- | -------- | ---------- | ---------- | ---------- | ----------- |
| 14  | Forky    | 待定         | 待定         | 待定         | 待定          |
| 13  | Trixie   | 2025-08-09 | 待定         | 待定         | 待定          |
| 12  | Bookworm | 2023-06-10 | 2026-06-10 | 2028-06-30 | 2033-06-30  |
| 11  | Bullseye | 2021-08-14 | 2024-08-14 | 2026-08-31 | 2031-06-30  |


**优选基础镜像：**

- `dunglas/frankenphp:1.11-php8.5-trixie`    159.09 - 197.76 MB
- `dunglas/frankenphp:1.11-php8.5-bookworm` 161.86 - 197.26 MB
- `dunglas/frankenphp:1.11-php8.4-trixie`    156.96 - 193.52 MB
- `dunglas/frankenphp:1.11-php8.4-bookworm`  159.74 - 193.52 MB
- `dunglas/frankenphp:1.11-php8.3-trixie`      153.18 - 189.7 MB
- `dunglas/frankenphp:1.11-php8.3-bookworm`  155.95 - 189.18 MB
- `dunglas/frankenphp:1.11-php8.5-alpine`     75.52 - 80.18 MB
- `dunglas/frankenphp:1.11-php8.4-alpine`     73.31 - 76.43 MB
- `dunglas/frankenphp:1.11-php8.3-alpine`     69.37 - 72.09 MB

---

## 镜像一览


| 镜像 Tag              | 运行时                          | 说明                                   |
| ------------------- | ---------------------------- | ------------------------------------ |
| `php8.5-frankenphp` | PHP 8.5 + Caddy (FrankenPHP) | 最新 PHP 8.5，HTTP/3、Worker 模式，适合生产与高并发 |
| `php8.4-frankenphp` | PHP 8.4 + Caddy (FrankenPHP) | PHP 8.4，HTTP/3、Worker 模式             |
| `php8.3-frankenphp` | PHP 8.3 + Caddy (FrankenPHP) | PHP 8.3 长期支持                         |
| `php8.3-apache`     | PHP 8.3 + Apache             | 传统 Apache 栈，兼容常见虚拟主机/面板              |
| `php8.2-apache`     | PHP 8.2 + Apache             | Apache + PHP 8.2                     |
| `php8.1-apache`     | PHP 8.1 + Apache             | Apache + PHP 8.1，兼容旧环境               |


- **FrankenPHP 镜像**：基于 `dunglas/frankenphp`，内置 Caddy、支持 HTTP/2/3、早期 hints、WP-CLI；扩展通过 `install-php-extensions` 安装。
- **Apache 镜像**：基于 `php:*-apache`，带 mod_rewrite、remoteip，使用 Debian 官方源构建。

---

## Laravel 基础镜像（yao3060/laravel）

用于运行 **Laravel Octane (FrankenPHP)** 的基础镜像，预装 redis、pdo_mysql、bcmath、gd、zip、pcntl、intl、fileinfo 等扩展及生产用 php.ini/opcache 配置。生产镜像可基于该镜像 `COPY` 应用代码后以 `php artisan octane:frankenphp` 启动。


| 镜像 Tag              | 说明                                               |
| ------------------- | ------------------------------------------------ |
| `php8.4-frankenphp` | PHP 8.4 + FrankenPHP（Alpine），Laravel Octane 基础镜像 |


```bash
docker build --tag yao3060/laravel:php8.4-frankenphp  ./laravel-php-8.4-frankenphp
docker push yao3060/laravel:php8.4-frankenphp
```

---

## Wordpress 基础镜像（yao3060/wordpress）

## php8.5-frankenphp

PHP 8.5 + Caddy（FrankenPHP），支持 HTTP/3、Worker 模式；扩展含 imagick 3.8.1、redis、pcntl，含 WP-CLI 与默认 MySQL client。

```bash
docker build --tag yao3060/wordpress:php8.5-frankenphp  ./wordpress-php-8.5-frankenphp
docker push yao3060/wordpress:php8.5-frankenphp
```

## php8.4-frankenphp

PHP 8.4 + Caddy（FrankenPHP），支持 HTTP/3、Worker 模式；扩展含 imagick 3.8.1、redis、pcntl，含 WP-CLI 与默认 MySQL client。

```bash
docker build --tag yao3060/wordpress:php8.4-frankenphp  ./wordpress-php-8.4-frankenphp
docker push yao3060/wordpress:php8.4-frankenphp
```

## php8.3-frankenphp

PHP 8.3 + Caddy（FrankenPHP），特性同 8.4 版；扩展含 imagick 3.7.0、redis、pcntl，含 WP-CLI 与 default-mysql-client。

```bash
docker build --tag yao3060/wordpress:php8.3-frankenphp  ./wordpress-php-8.3-frankenphp
docker push yao3060/wordpress:php8.3-frankenphp
```

## php8.3-apache

PHP 8.3 + Apache 2.4，mod_rewrite、mod_remoteip 已启用；扩展含 imagick（PECL 手动安装）、redis，适合与反向代理/面板搭配。

```bash
docker build --tag yao3060/wordpress:php8.3-apache  ./wordpress-php-8.3-apache
docker push yao3060/wordpress:php8.3-apache
```

## php8.2-apache

PHP 8.2 + Apache，与 8.3-apache 结构一致，适合需要 PHP 8.2 的项目。

```bash
docker build --tag yao3060/wordpress:php8.2-apache  ./wordpress-php-8.2-apache
docker push yao3060/wordpress:php8.2-apache
```

## php8.1-apache

PHP 8.1 + Apache，与 8.2/8.3-apache 结构一致，兼容旧版 PHP 需求。

```bash
docker build --tag yao3060/wordpress:php8.1-apache ./wordpress-php-8.1-apache
docker push yao3060/wordpress:php8.1-apache
```

## CI/CD via GitHub Actions

本仓库已配置 GitHub Actions，自动构建并推送以下镜像到 Docker Hub：

**WordPress（yao3060/wordpress）**

- `yao3060/wordpress:php8.5-frankenphp`（上下文：`wordpress-php-8.5-frankenphp/`）
- `yao3060/wordpress:php8.4-frankenphp`（上下文：`wordpress-php-8.4-frankenphp/`）
- `yao3060/wordpress:php8.3-frankenphp`（上下文：`wordpress-php-8.3-frankenphp/`）
- `yao3060/wordpress:php8.3-apache`（上下文：`wordpress-php-8.3-apache/`）
- `yao3060/wordpress:php8.2-apache`（上下文：`wordpress-php-8.2-apache/`）
- `yao3060/wordpress:php8.1-apache`（上下文：`wordpress-php-8.1-apache/`）

**Laravel 基础镜像（yao3060/laravel）**

- `yao3060/laravel:php8.4-frankenphp`（上下文：`laravel-php-8.4-frankenphp/`）

触发条件：

- 向 `main` 分支推送且涉及上述目录的变更
- 手动触发（Workflow Dispatch）

必需的仓库 Secrets：

- `DOCKERHUB_USERNAME`：Docker Hub 用户名（例如 `yao3060`）
- `DOCKERHUB_TOKEN`：Docker Hub Access Token（到 Docker Hub -> Security -> New Access Token 创建）

工作流文件：`.github/workflows/docker-publish.yml`

构建说明：

- 使用 Buildx 构建并推送多架构镜像：`linux/amd64` 与 `linux/arm64`
- 推送到 `yao3060/wordpress` 仓库，对应各自 tag（见上）

