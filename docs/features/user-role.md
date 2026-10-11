# Fitur: Role dan Status User

## Tujuan

Menambahkan role dasar dan status aktif pada tabel `users` untuk mendukung hak akses aplikasi perpustakaan.

## Aktor dan nilai role

| Nilai database | Keterangan |
|---|---|
| `admin` | Mengelola seluruh sistem |
| `officer` | Mengelola buku, anggota, dan transaksi |
| `student` | Melihat katalog dan riwayat miliknya |

Kolom `is_active` digunakan untuk mengaktifkan atau menonaktifkan akun tanpa menghapus data user.

## File yang dibuat

```text
database/migrations/2026_10_11_002337_add_role_and_is_active_to_users_table.php
```

## Perintah pembuatan

```bash
php artisan make:migration add_role_and_is_active_to_users_table --table=users
```

## Script migration

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::table('users', function (Blueprint $table) {
            $table->string('role', 20)
                ->default('student')
                ->after('password');

            $table->boolean('is_active')
                ->default(true)
                ->after('role');
        });
    }

    public function down(): void
    {
        Schema::table('users', function (Blueprint $table) {
            $table->dropColumn(['role', 'is_active']);
        });
    }
};
```

## Cara menjalankan

Jalankan dari root project, yaitu folder yang berisi file `artisan`:

```bash
php artisan migrate
```

## Cara memeriksa

```bash
php artisan migrate:status
```

Periksa juga melalui phpMyAdmin bahwa tabel `users` memiliki kolom:

- `role`
- `is_active`

## Perubahan model

Posisi file: `app/Models/User.php`

Tambahkan `role` dan `is_active` ke attribute `Fillable`:

```php
#[Fillable(['name', 'email', 'password', 'role', 'is_active'])]
```

Tambahkan cast boolean pada method `casts()`:

```php
protected function casts(): array
{
    return [
        'email_verified_at' => 'datetime',
        'password' => 'hashed',
        'is_active' => 'boolean',
    ];
}
```

## Perubahan seeder

Posisi file: `database/seeders/DatabaseSeeder.php`

Seeder dibuat idempoten menggunakan `updateOrCreate`, sehingga aman dijalankan ulang tanpa membuat user duplikat:

```php
use Illuminate\Support\Facades\Hash;

public function run(): void
{
    $users = [
        [
            'name' => 'Administrator Perpustakaan',
            'email' => 'admin@perpustakaan.test',
            'role' => 'admin',
        ],
        [
            'name' => 'Petugas Perpustakaan',
            'email' => 'officer@perpustakaan.test',
            'role' => 'officer',
        ],
        [
            'name' => 'Siswa Contoh',
            'email' => 'student@perpustakaan.test',
            'role' => 'student',
        ],
    ];

    foreach ($users as $user) {
        User::updateOrCreate(
            ['email' => $user['email']],
            [
                'name' => $user['name'],
                'role' => $user['role'],
                'is_active' => true,
                'password' => Hash::make('password'),
            ],
        );
    }
}
```

Password `password` hanya untuk development lokal. Ganti dengan password aman atau mekanisme reset password sebelum production.

## Cara menjalankan seeder

```bash
php artisan db:seed
```

Perintah ini menambahkan atau memperbarui tiga akun role. Jangan menggunakan `migrate:fresh --seed` pada database yang berisi data penting karena perintah tersebut menghapus tabel dan data.

## Cara menguji

```bash
php artisan test
php artisan tinker --execute="echo App\\Models\\User::select('email', 'role', 'is_active')->get();"
```

## Dampak dan aturan lanjutan

- User baru otomatis memiliki role `student` dan status aktif.
- Hak akses belum boleh dianggap selesai hanya karena kolom role sudah ada.
- Tahap berikutnya harus menambahkan role ke model, seeder, middleware atau policy, dan test.
- Jangan menggunakan role dari input browser tanpa validasi server.

## Status

- [x] Migration dibuat.
- [x] Migration dijalankan oleh developer.
- [x] Field ditambahkan ke `User` model.
- [x] User admin, officer, dan student ditambahkan ke seeder.
- [ ] Authorization diuji.
