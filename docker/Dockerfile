# PHP-FPMの設定
FROM php:8.1-fpm-alpine as php-fpm

# 作業ディレクトリの設定
WORKDIR /var/www/html

# 依存パッケージのインストール
RUN apk add --no-cache \
    curl \
    libpng-dev \
    libjpeg-turbo-dev \
    libwebp-dev \
    zlib-dev \
    libxpm-dev \
    freetype-dev \
    libzip-dev \
    oniguruma-dev

# PHP拡張機能のインストール
RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install gd pdo pdo_mysql mbstring zip exif pcntl

# Composerのインストール
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Laravelプロジェクトのコピー
COPY . /var/www/html

# Composerを使った依存関係のインストール
RUN composer install --optimize-autoloader --no-dev

# NginxとPHP-FPMを含む最終的なイメージの構築
FROM nginx:alpine as nginx

# PHP-FPM層からコードをコピー
COPY --from=php-fpm /var/www/html /var/www/html

# Nginx設定ファイルのコピー
COPY ./docker/nginx.conf /etc/nginx/nginx.conf

# ポート80を開放
EXPOSE 80

# Nginxをフォアグラウンドで起動
CMD ["nginx", "-g", "daemon off;"]
