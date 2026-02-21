ARG PHP_VERSION=8.5
ARG USER=www-data

FROM dunglas/frankenphp:1.11-php${PHP_VERSION}-bookworm

ARG ENVIRONMENT

# persistent dependencies (runtime only; build deps handled by install-php-extensions)
RUN set -eux; \
	apt-get update; \
	apt-get install -y --no-install-recommends \
		ghostscript \
		default-mysql-client \
	; \
	rm -rf /var/lib/apt/lists/*

# PHP extensions for WordPress (https://make.wordpress.org/hosting/handbook/handbook/server-environment/#php-extensions)
# FrankenPHP image has install-php-extensions built-in; it installs system deps and cleans up automatically
RUN install-php-extensions \
		bcmath \
		exif \
		gd \
		intl \
		imagick-3.8.1 \
		mysqli \
		pcntl \
		redis \
		zip

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

# https://wordpress.org/support/article/editing-wp-config-php/#configure-error-logging

RUN { \
    # https://www.php.net/manual/en/errorfunc.constants.php
    # https://github.com/docker-library/wordpress/issues/420#issuecomment-517839670
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
    } > $PHP_INI_DIR/conf.d/wp.ini


WORKDIR /var/www/html

# Adding WordPress CLI
RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && \
    chmod +x wp-cli.phar && \
    mv wp-cli.phar /usr/local/bin/wp && \
    mkdir -p /var/www/.wp-cli

COPY ./Caddyfile /etc/caddy/Caddyfile
