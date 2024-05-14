FROM nginx:1.26.0-alpine

# PHPパッケージをインストールし、PHPへのシンボリックリンクを作成する
RUN apk update && \
    apk upgrade && \
    apk add --no-cache \
        curl \
        php83 \
        php83-fpm \
        php83-pdo \
        php83-pdo_mysql \
        php83-mbstring \
        php83-openssl \
        php83-json \
        php83-curl \
        php83-gd \
        php83-xml \
        php83-zip \
        php83-exif \
        php83-iconv \
        php83-phar \
        php83-intl \
        php83-bcmath \
        php83-dom \
        php83-xmlreader \
        php83-xmlwriter \
        php83-session \
        php83-fileinfo \
        php83-tokenizer && \
    rm -f /usr/bin/php && \
    ln -s /usr/bin/php83 /usr/bin/php

# 設定ファイルをコピーする
COPY ./docker/php-fpm.conf /etc/php83/php-fpm.conf
COPY ./docker/nginx.conf /etc/nginx/nginx.conf

# Composerをインストールする
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# 作業ディレクトリを設定する
WORKDIR /var/www/html

# Laravelプロジェクトをコピーする
COPY . /var/www/html

# Composerの依存関係をインストールする
RUN composer install --optimize-autoloader --no-dev

# supervisordをインストールする
RUN apk add --no-cache supervisor

# supervisordの設定ファイルをコピーする
COPY ./docker/supervisord.conf /etc/supervisord.conf

# ストレージディレクトリの所有者をnginxに変更する
RUN chown -R nginx:nginx storage/

# ポート80を公開する
EXPOSE 80

# NginxとPHP-FPMを実行するコマンド
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
