# WordPress PHP 8.4 Apache 镜像说明

基于 `php:8.4-apache` 构建，适用于 WordPress 运行环境，内置常用 PHP 扩展、Apache 模块和运维工具。

## 基础信息

- 基础镜像：`php:8.4-apache`
- 挂载目录：`/var/www/html`（已声明为 `VOLUME`）

## 已安装系统依赖

- `ghostscript`（PDF 预览渲染）
- `libheif-plugin-aomenc`、`libheif-plugin-x265`（AV1/HEVC 编码支持）
- `default-mysql-client`（MySQL 客户端）

## 已安装 PHP 扩展

通过 `docker-php-ext-install` / PECL / 扩展安装器启用：

- `bcmath`
- `exif`
- `gd`（开启 `avif` / `freetype` / `jpeg` / `webp`）
- `intl`
- `mysqli`
- `zip`
- `imagick`（`3.8.1`）
- `pcntl`
- `redis`

## 已内置工具

- Composer（从 `composer:2.7.7` 拷贝）
- WP-CLI（命令为 `wp`）

## Apache 配置

已启用模块：

- `rewrite`
- `expires`
- `remoteip`

并额外配置了：

- `RemoteIPHeader X-Forwarded-For`
- 常见内网段作为 `RemoteIPInternalProxy`
- Apache 日志中的客户端地址由 `%h` 替换为 `%a`

## PHP 运行参数

### OPcache 建议配置

- `opcache.memory_consumption=128`
- `opcache.interned_strings_buffer=8`
- `opcache.max_accelerated_files=4000`
- `opcache.revalidate_freq=2`

### 错误日志配置

- `display_errors=Off`
- `display_startup_errors=Off`
- `log_errors=On`
- `error_log=/dev/stderr`

## 构建镜像

在当前目录执行：

```bash
docker build -t wordpress:php8.4-apache .
```

## 运行示例

```bash
docker run -d \
  --name wordpress-php84 \
  -p 8080:80 \
  -v "$PWD/html:/var/www/html" \
  wordpress:php8.4-apache
```

访问：`http://localhost:8080`
