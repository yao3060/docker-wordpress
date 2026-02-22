########################################################################################
# FrankenPHP stage: run Laravel Octane (FrankenPHP)
# 当前用的 Alpine 更小，对多数 Laravel 应用没问题
# 官方 FrankenPHP 文档更推荐 Debian (bookworm)，若遇到扩展或兼容问题，可改为 `dunglas/frankenphp:1.11-php8.4-bookworm`
########################################################################################
FROM dunglas/frankenphp:1.11-php8.4-alpine AS frankenphp


LABEL org.opencontainers.image.title="Laravel Octane + FrankenPHP (PHP 8.4) base image"

# Install additional PHP extensions
ADD --chmod=0755 https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN install-php-extensions \
  redis \
  pdo_mysql \
  bcmath \
  gd \
  zip \
  pcntl \
  intl \
  fileinfo

RUN cp $PHP_INI_DIR/php.ini-production $PHP_INI_DIR/php.ini

# set recommended PHP.ini settings
# see https://secure.php.net/manual/en/opcache.installation.php
RUN set -eux; \
    docker-php-ext-enable opcache; \
    { \
    echo 'opcache.memory_consumption=128'; \
    echo 'opcache.interned_strings_buffer=8'; \
    echo 'opcache.max_accelerated_files=4000'; \
    echo 'opcache.revalidate_freq=60'; \
    echo 'opcache.enable_cli=1'; \
    } > $PHP_INI_DIR/conf.d/opcache-recommended.ini

RUN { \
    # https://www.php.net/manual/en/errorfunc.constants.php
    echo 'error_reporting = E_ERROR | E_WARNING | E_PARSE | E_CORE_ERROR | E_CORE_WARNING | E_COMPILE_ERROR | E_COMPILE_WARNING | E_RECOVERABLE_ERROR'; \
    echo 'display_errors = Off'; \
    echo 'display_startup_errors = Off'; \
    echo 'log_errors = On'; \
    echo 'error_log = /dev/stderr'; \
    echo 'log_errors_max_len = 1024'; \
    echo 'ignore_repeated_errors = On'; \
    echo 'ignore_repeated_source = Off'; \
    echo 'html_errors = Off'; \
    } > $PHP_INI_DIR/conf.d/error-logging.ini

RUN { \
    echo '[php]'; \
      echo 'upload_max_filesize = 512M'; \
      echo 'post_max_size = 512M'; \
      } > $PHP_INI_DIR/conf.d/laravel.ini

WORKDIR /app
