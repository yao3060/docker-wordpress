# README

docker hub page: [https://hub.docker.com/repository/docker/yao3060/wordpress/general](https://hub.docker.com/repository/docker/yao3060/wordpress/general)

## php8.4 + frankenphp

```bash
docker build --tag yao3060/wordpress:php8.4-frankenphp  ./8.4-frankenphp
docker push yao3060/wordpress:php8.4-frankenphp
```

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

## CI/CD via GitHub Actions

本仓库已配置 GitHub Actions，自动构建并推送以下镜像到 Docker Hub：

- `yao3060/wordpress:php8.4-frankenphp`（上下文：`8.4-frankenphp/`）
- `yao3060/wordpress:php8.3-frankenphp`（上下文：`8.3-frankenphp/`）
- `yao3060/wordpress:php8.3-apache`（上下文：`8.3-apache/`）
- `yao3060/wordpress:php8.2-apache`（上下文：`8.2-apache/`）
- `yao3060/wordpress:php8.1-apache`（上下文：`8.1-apache/`）

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
