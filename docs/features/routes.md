# Standar Route Aplikasi Perpustakaan

## Tujuan

Menjaga `routes/web.php` dan `routes/api.php` tetap rapi, mudah dipahami, aman, dan mudah diuji.

## Aturan utama

- Route hanya menjadi penghubung URL ke controller.
- Logika bisnis berada di Service.
- Validasi berada di Form Request.
- Authorization berada di middleware atau Policy.
- Query database tidak ditulis di route closure.
- Semua route penting memiliki route name.
- Middleware dikelompokkan agar tidak berulang.
- CRUD menggunakan `Route::resource()` jika sesuai.

## Struktur route yang disarankan

```text
routes/
├── web.php       # halaman web, panel publik, dan redirect
├── api.php       # endpoint API jika dibutuhkan
├── console.php   # command Artisan
└── channels.php  # broadcast channel jika dibutuhkan
```

## Contoh route aplikasi

Posisi file: `routes/web.php`

```php
<?php

use App\Http\Controllers\BookController;
use App\Http\Controllers\DashboardController;
use Illuminate\Support\Facades\Route;

Route::get('/', fn () => to_route('dashboard'));

Route::middleware('auth')->group(function () {
    Route::get('/dashboard', DashboardController::class)
        ->name('dashboard');

    Route::resource('books', BookController::class)
        ->except(['show']);
});
```

## Dokumentasi setiap perubahan

Setiap perubahan route harus dicatat di dokumentasi fitur terkait menggunakan format:

| Field | Isi |
|---|---|
| Tanggal | Tanggal perubahan |
| Route name | Nama route |
| Method dan URL | Contoh `GET /books` |
| Controller/action | Tujuan request |
| Middleware | `auth`, `role:admin`, dan lainnya |
| Role | Role yang diizinkan |
| Alasan | Mengapa route dibuat/diubah |
| Testing | Skenario yang diperiksa |

## Pemeriksaan setelah perubahan

```bash
php artisan route:list
php artisan test
```

## Larangan

- Jangan menaruh proses peminjaman atau pengembalian langsung di `web.php`.
- Jangan membuat URL berbeda untuk fungsi yang sama tanpa alasan.
- Jangan menggunakan nama route yang tidak konsisten.
- Jangan melindungi halaman hanya dengan menyembunyikan link di Blade.
