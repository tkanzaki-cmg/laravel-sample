FROM nginx:1.25.2-alpine

# PHPパッケージをインストールし、PHPへのシンボリックリンクを作成する
RUN apk update && \
    apk upgrade && \
    apk add --no-cache \
        curl \
        php81 \
        php81-fpm \
        php81-pdo \
        php81-pdo_mysql \
        php81-mbstring \
        php81-openssl \
        php81-json \
        php81-curl \
        php81-gd \
        php81-xml \
        php81-zip \
        php81-exif \
        php81-iconv \
        php81-phar \
        php81-intl \
        php81-bcmath \
        php81-dom \
        php81-xmlreader \
        php81-xmlwriter \
        php81-session \
        php81-fileinfo \
        php81-tokenizer && \
    rm -f /usr/bin/php && \
    ln -s /usr/bin/php81 /usr/bin/php

# 設定ファイルをコピーする
COPY ./docker/php-fpm.conf /etc/php81/php-fpm.conf
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
