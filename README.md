## ローカル環境
- PHP 8.3.7
- Docker version 25.0.4

## インストール
- ローカル
```
cp .env.example .env
php artisan key:generate
docker compose up
```

- docker
```
cp .env.example .env
docker compose up
docker compose exec panel-supply-backend-app php artisan key:generate
```

## 起動
- ローカル
```
php artisan serve
```

- docker
```
docker compose exec panel-supply-backend-app php artisan serve
```

## アクセス
```
http://localhost:8000
```

## フォーマット
- ローカル
```
composer format
```

- docker
```
docker compose exec panel-supply-backend-app composer -- format
```
