# 7. Standar Dokumentasi dan Clean Code

## Dokumentasi setiap fitur

Simpan fitur besar di `docs/features/<nama-fitur>.md` menggunakan format berikut:

```text
Nama fitur:
Tujuan:
Aktor yang dapat menggunakan:
Route:
Controller:
Request/validasi:
Model/tabel:
Alur sukses:
Alur gagal:
Skenario testing:
Catatan perubahan:
```

Setiap script atau coding baru wajib didokumentasikan sebelum lanjut ke fitur berikutnya. Minimal dokumentasi harus mencantumkan:

- Tujuan perubahan.
- Posisi file.
- Perintah generator atau instalasi jika ada.
- Kode yang dimasukkan.
- Cara menjalankan.
- Cara menguji.
- Status pekerjaan dan catatan error.

Contoh dokumentasi implementasi tersedia di [features/user-role.md](features/user-role.md).

## Penamaan

- Class: `PascalCase`, contoh `LoanService`.
- Method dan variable: `camelCase`, contoh `calculateFine`.
- Tabel dan kolom: `snake_case`, contoh `loan_items`.
- Route name: dot notation, contoh `loans.store`.

## Clean code

- Satu method memiliki satu tanggung jawab utama.
- Nama variable harus menjelaskan isi atau tujuan.
- Hindari magic number dan status string yang tersebar.
- Hindari duplikasi query.
- Gunakan dependency injection.
- Controller tipis; view hanya menampilkan data.
- Filament Resource hanya mengatur kebutuhan panel; aturan bisnis kompleks diletakkan di Service Laravel.
- Setiap Filament Resource wajib memiliki aturan authorization yang sesuai.
- Tangani exception dengan pesan aman dan log detail untuk developer.
- Jangan mengedit file apa pun di dalam `vendor/`, termasuk source code Laravel, Filament, Livewire, atau package lain.

## Aturan vendor dan package

- Folder `vendor/` adalah hasil instalasi Composer dan harus dianggap read-only.
- Jangan memperbaiki bug package dengan mengedit file di `vendor/`.
- Jangan menaruh class, view, config, atau helper custom di `vendor/`.
- Jika perlu mengubah perilaku package, gunakan extension point resmi, config publish, middleware, policy, event, service provider, atau plugin.
- Jika package perlu diperbaiki, laporkan upstream atau gunakan fork/package internal yang terdokumentasi.
- Setelah `composer update` atau instalasi ulang, semua perubahan di `vendor/` akan hilang.
- Custom code harus berada di `app/`, `config/`, `database/`, `resources/`, `routes/`, atau `tests/`.

## Standar coding Filament

- Nama Resource mengikuti model, contoh `BookResource` untuk model `Book`.
- Form dan table Filament tidak boleh menjadi satu-satunya tempat validasi bisnis.
- Gunakan Policy untuk authorization record dan action.
- Gunakan `->relationship()` untuk field relasi jika sesuai, bukan query manual berulang.
- Gunakan eager loading pada tabel atau query yang menampilkan relasi.
- Dokumentasikan setiap Resource di `docs/features/<nama-fitur>.md`.
- Uji akses sebagai admin, officer, dan student.
- Jangan membuat custom panel atau plugin sebelum kebutuhan dan dampaknya terdokumentasi.

## Standar route dan `web.php`

- Simpan route web di `routes/web.php` dan route API di `routes/api.php`.
- Letakkan seluruh import controller dan class di bagian atas file.
- Gunakan route name dengan pola konsisten, contoh `books.index`, `loans.store`, dan `dashboard`.
- Gunakan `Route::middleware()->group()` untuk route dengan perlindungan yang sama.
- Gunakan `Route::resource()` untuk CRUD standar jika seluruh action memang diperlukan.
- Gunakan `Route::controller()` jika beberapa route memakai controller yang sama.
- Route hanya mengatur URL, middleware, controller, dan nama route.
- Jangan menaruh query database, perhitungan denda, perubahan stok, atau aturan bisnis di route closure.
- Closure hanya boleh digunakan untuk route sederhana seperti health check atau redirect.
- Hindari route duplikat dan URL hard-code di Blade; gunakan `route()` atau `to_route()`.
- Route sensitif wajib dilindungi `auth`, policy, atau middleware role yang sesuai.
- Periksa route dengan `php artisan route:list` setelah perubahan.

### Contoh struktur route yang rapi

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

## Dokumentasi perubahan route

Setiap penambahan, pengubahan, atau penghapusan route wajib dicatat di `docs/features/<nama-fitur>.md` dengan informasi:

```text
Tanggal perubahan:
Route name:
HTTP method dan URL:
Controller/action:
Middleware:
Role yang diizinkan:
Alasan perubahan:
Skenario testing:
```

Setelah mengubah route, dokumentasikan output atau pemeriksaan berikut:

```bash
php artisan route:list
php artisan test
```

## Commit

```text
feat: add book management
fix: prevent negative book stock
refactor: move loan logic to service
test: add loan return scenarios
docs: update database design
chore: update dependencies
```

## Testing minimum

Unit test untuk perhitungan denda, jatuh tempo, dan nomor transaksi.

Feature test untuk login, hak akses, pembuatan peminjaman, stok berkurang, pengembalian, stok bertambah, buku tanpa stok, dan denda keterlambatan.

## Keamanan minimum

- Gunakan autentikasi dan CSRF Laravel.
- Lindungi endpoint dengan middleware atau policy.
- Jangan percaya input dari browser.
- Gunakan Eloquent atau query builder.
- Jangan menampilkan stack trace pada production.
- Simpan rahasia di environment variable.
- Batasi upload berdasarkan MIME type, ukuran, dan lokasi penyimpanan.
