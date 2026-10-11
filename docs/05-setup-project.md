# 5. Setup Project

## Prasyarat

PHP 8.3+, Composer, MySQL, Node.js, npm, Git, dan editor kode. Project menggunakan Laravel 13 dan Filament 5. Catat semua versi di dokumentasi project.

## Membuat project Laravel

Jalankan dari folder workspace:

```bash
composer create-project laravel/laravel perpustakaan
cd perpustakaan
php artisan serve
```

Jika project akan langsung dibuat di folder workspace saat ini, pastikan tidak menimpa file yang sudah ada.

## Database MySQL

```sql
CREATE DATABASE perpustakaan_sekolah
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
```

Salin `.env.example` menjadi `.env`, lalu isi secara lokal. Posisi file: `.env`.

```dotenv
APP_NAME="Perpustakaan Sekolah"
APP_ENV=local
APP_DEBUG=true
APP_URL=http://127.0.0.1:8000

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=perpustakaan_sekolah
DB_USERNAME=root
DB_PASSWORD=
```

Jangan commit `.env`.

## Key, migration, dan asset

```bash
php artisan key:generate
php artisan migrate
php artisan storage:link
npm install
npm run dev
```

## Install Filament

Jalankan dari root project Laravel:

```bash
composer require filament/filament:"^5.0"
php artisan filament:install --panels
npm install
npm run build
```

Filament akan membuat panel provider di:

```text
app/Providers/Filament/AdminPanelProvider.php
```

Panel admin dapat diakses melalui `/admin`. Gunakan Filament untuk admin dan petugas, sedangkan halaman katalog publik tetap dapat menggunakan Blade.

## Git awal

```bash
git init
git add .
git commit -m "chore: initialize laravel project"
```

Periksa agar `.env`, password, token, dan file rahasia tidak ikut commit.

## Struktur folder kerja

```text
app/
├── Http/
│   ├── Controllers/
│   ├── Requests/
│   └── Middleware/
├── Models/
└── Services/
database/
├── factories/
├── migrations/
└── seeders/
docs/
resources/views/
├── layouts/
├── components/
├── dashboard/
├── books/
├── categories/
├── members/
├── loans/
└── reports/
routes/
├── web.php
└── api.php
tests/
├── Feature/
└── Unit/
```
