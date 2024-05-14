## 推奨ローカル環境
- PHP8.1
- Composer version 2.6.6

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
