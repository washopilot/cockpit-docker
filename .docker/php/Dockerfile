FROM php:8.1.20-fpm-alpine3.18
# persistent / runtime deps
RUN apk add --no-cache \
		acl \
		fcgi \
		file \
		gettext \
		git \
		ttf-freefont \
		fontconfig \
		dbus \
		freetype-dev \
		libjpeg-turbo-dev \
		libpng-dev

RUN set -eux; \
	apk add --no-cache --virtual .build-deps \
		$PHPIZE_DEPS \
		icu-dev \
		libzip-dev \
		zlib-dev \
		oniguruma-dev \
	; \
	\
	docker-php-ext-configure zip; \
	docker-php-ext-configure gd --with-freetype --with-jpeg ;\
	docker-php-ext-install -j$(nproc) \
		intl \
		pdo_mysql \
		zip \
		gd \
		exif \
		pdo \
		# iconv \
		pcntl \
		mbstring \
		fileinfo \
		posix \
	; \
	# pecl clear-cache; \
	docker-php-ext-enable \
		# apcu \
		opcache \
	;
COPY .docker/php/cockpit.ini /usr/local/etc/php/conf.d/cockpit.ini
COPY ./cockpit-core/ /var/www/html